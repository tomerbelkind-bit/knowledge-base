---
title: "GVP vs. SchNet vs. EGNN"
type: comparison
updated: "2026-03-22"
topics:
  - Enzyme-Substrate-Alignment
papers:
  - DrugCLIP
  - EnzymeCAGE
tags:
status: 
related:
---

# GVP vs. SchNet vs. EGNN

| Feature             | **SchNet** | **GVP-GNN**            | **EGNN**                |
| ------------------- | ---------- | ---------------------- | ----------------------- |
| Geometry input      | Distances  | Distances + directions | Distances + coordinates |
| Vector features     | ❌          | ✅ explicit           | ❌ implicit              |
| Coordinate updates  | ❌          | ❌                    | ✅                       |
| Rotation handling   | Invariant  | Equivariant            | Equivariant             |
| Direction awareness | ❌          | ✅                    | ✅ (via coords)          |
| Complexity          | Low        | Medium                 | Medium                  |
| Interpretability    | Low        | Medium (vectors)       | Medium (coords)         |

## Core conceptual difference

**SchNet** - “Ignore direction, just use distances”
**GVP** - “Store direction explicitly as vectors”
**EGNN** - “Don’t store direction — let it emerge from coordinates”

## GVP vs EGNN

**GVP**

- Keeps vector features
- Geometry lives in: feature space

👉 “direction is a feature”

**EGNN**

- Keeps coordinates
- Geometry lives in: the positions themselves

👉 “direction comes from positions”

## Strengths & weaknesses

**SchNet**:
- Simple
- Stable 
- {#d65656:Misses direction}

**GVP**:
- Explicit geometric features
- Strong for proteins
- {#d65656:Heavier representation}

**EGNN**:
- Elegant (no vector features needed)
- Updates geometry dynamically
- Works great for molecular dynamics and generative models
- {#d65656:Less explicit control over geometry}

## Mental Model

- SchNet → scalar world
- GVP → vector world
- EGNN → coordinate world

## Trends

Many newer papers prefer EGNN because it gives similar geometric power with a simpler, more flexible, and more scalable design.
