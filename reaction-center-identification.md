---
title: "Reaction Center Identification"
type: other
updated: "2026-03-24"
topics:
  - Molecular Representation Learning
  - Enzyme-Substrate-Alignment
papers:
  - EnzymeCAGE
tags:
status: open
related:
---

# Reaction Center Identification (Substrate/Product) 

**Input**: atom types, 2D graph, atom features (format charge, valence, etc.) 
**Output**: reaction center weights / mask

## Key insight for identifying reaction center

There is no learning for identifying the reaction center, and this is actually a **design choise**:
- Injects domain knowledge about chemical reactions
- Focuses the model on chemically relevant atoms
- Reduces the burden on the model to infer reaction centers
- Trades off end-to-end learning for inductive bias

## Molecule representation (2D)

Each molecule is represented as a graph with:
- Atoms (nodes): element type (C, N, O, …), formal charge, chirality, valence
- Bonds (edges): bond type (single, double, aromatic, etc.)

These typically come from:
- SMILES / reaction SMILES
- Parsed using chemistry tools like RDKit

## Atom mapping 

To compare substrate ↔ product, the model needs atom mapping - which atom in the substrate corresponds to which atom in the product.
This usually comes from:
- Reaction datasets (e.g., USPTO)
- or atom-mapping algorithms

## Identify reaction center

Inputs: substrate graph, product graph, atom mapping
Output: atoms that undergo changes in bonding, charge, or chirality

Method:
- Graph comparison (rule-based)
- Detect: bond changes (formed/broken), bond order changes, charge changes, chirality changes

Reaction center identification relies on **2D graph information**, not on 3D geometry.

## Assign weights

- Atoms in reaction center → higher weights
- Others → lower weights

Manually defined heuristic / preprocessing




