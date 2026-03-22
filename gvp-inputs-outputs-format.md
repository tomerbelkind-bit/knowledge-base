---
title: "GVP Inputs & Outputs Format"
type: paper-deep-dive
updated: "2026-03-21"
topics:
  - Enzyme-Substrate-Alignment
papers:
  - GVP
tags:
status: 
related:
---

# GVP Inputs & Outputs Format

Each protein is a JSON entry with two fields:
```
{
    "seq": "ACDEFGHIK...",
    "coords": 
    {
      "N":  [[x,y,z], ...],
      "CA": [[x,y,z], ...],
      "C":  [[x,y,z], ...],
      "O":  [[x,y,z], ...]
    }
 }
```

The coordinates are absolute Cartesian coordinates in the standard PDB frame (units: Ångströms)

Four backbone atoms per residue (N, Cα, C, O), each as a 3D coordinate.

## After parse_batch (datasets.py:73)

The dict is stacked into a dense tensor X of shape [B, L, 4, 3]:
  - B = batch size (number of proteins)
  - L = max sequence length in the batch (padded with NaN → zeroed)
  - 4 = atoms in order: N, CA, C, O
  - 3 = x, y, z

Also produced: S [B, L] (integer amino acid indices) and mask [B, L] (1.0 for real residues, 0.0 for padding).

## Inside StructuralFeatures.call (models.py:420)

X [B, L, 4, 3] is featurized into a graph. The steps:

1. **k-NN graph** — built on Cα only (X[:,:,1,:]). Produces E_idx [B, L, K] (neighbor indices, k=30).
2. **Edge features E** — per neighbor pair:
    - {cyan:E_directions}: unit vectors from node to each neighbor (3D vectors) → [B, L, K, 3]
    - {cyan:RBF}: 16 radial basis functions of pairwise Cα distance (0–20 Å) → [B, L, K, 16]
    - {cyan:E_positional}: sinusoidal positional encodings of sequence offset (i−j) → [B, L, K, 16]
3. **Node features V** — per residue:
    - {cyan:V_dihedrals}: φ, ψ, ω backbone dihedral angles as (cos, sin) pairs → 6 scalars
    - {cyan:V_orientations}: forward/backward Cα–Cα unit vectors → 2 vectors ([B, L, 3, 2])
    - {cyan:V_sidechains}: a pseudo-Cβ direction estimated from N, CA, C positions → 1 vector

Combined: V has 3 vector channels (sidechains + 2 orientations) and 6 scalar channels (dihedrals).

The packed V and E tensors are then embedded by linear GVP layers before entering the encoder.

**Summary**
```
JSON file
└-- parse_batch()  →  X [B, L, 4, 3],  S [B, L],  mask [B, L]
   └─- StructuralFeatures.call(X, mask)
      ├─- k-NN on Cα  →  E_idx [B, L, 30]
      |─- Node features V  (3 vectors + 6 scalars per residue)
      └─- Edge features E  (1 vector + 32 scalars per neighbor pair)
         └─- Encoder → Decoder → logits [B, L, 20]
```
