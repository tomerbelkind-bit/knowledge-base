---
title: "Uni-Mol vs. Uni-Mol3 comparison"
type: comparison
updated: "2026-03-19"
topics:
  - Enzyme-Substrate-Alignment
papers:
  - Uni-Mol
tags:
status: open
related:
---

# Uni-Mol vs. Uni-Mol3 comparison

While **Uni-Mol** was designed for individual molecules and protein pockets, **Uni-Mol3** (introduced in late 2025) represents a shift toward **multi-molecular systems** and chemical reactions.

The primary difference is their scope: Uni-Mol focuses on the "static" properties of a single molecule, while Uni-Mol3 focuses on the "dynamic" process of how multiple molecules interact during a chemical reaction.

---

## 1. Scope: Single vs. Multi-Molecular
* **Uni-Mol:** Designed as a "3D foundation model" for single molecules. Its primary job is to understand the geometry and properties of one entity at a time (or a ligand inside a static protein pocket).
* **Uni-Mol3:** Designed for **Organic Reaction Modeling**. It handles systems where multiple reactants, reagents, and catalysts coexist. It is built to model the breaking and forming of bonds, rather than just a stable 3D shape.

## 2. Architecture: Tokenization vs. Continuous Coordinates
* **Uni-Mol:** Uses a continuous **SE(3)-equivariant Transformer**. It processes raw $[x, y, z]$ coordinates directly and relies on pairwise distances to understand shape.
* **Uni-Mol3:** Introduces a **Mol-Tokenizer**. It quantizes 1D atomic features, 2D graphs, and 3D coordinates into **discrete tokens**. This creates a "3D-aware molecular language," allowing the model to treat chemical reactions almost like a translation task (Reactants $\rightarrow$ Products).

## 3. Pre-training Strategy
| Feature | Uni-Mol | Uni-Mol3 |
| :--- | :--- | :--- |
| **Data Focus** | 209M single-molecule conformations. | Hierarchical: Single molecules + Multi-molecule reactions. |
| **Learning Task** | Denoising 3D coordinates & Masked atom prediction. | **Reaction Pre-training:** Learning thermodynamic and kinetic principles. |
| **Output Goal** | Stable 3D structures and property scores. | Reaction products, yields, and retrosynthetic pathways. |

## 4. Downstream Tasks
* **Uni-Mol Tasks:** Solubility prediction, toxicity, 3D conformer generation, and protein-ligand docking.
* **Uni-Mol3 Tasks:** * **Product Prediction:** Given reactants and conditions, what is the result?
    * **Retrosynthesis:** Given a product, what were the starting materials?
    * **Reaction Condition Recommendation:** What temperature or catalyst is needed?
    * **Yield Prediction:** How much product will actually be created?


---

## Summary Table

| Feature | Uni-Mol | Uni-Mol3 |
| :--- | :--- | :--- |
| **Core Entity** | The Molecule | The Reaction |
| **Input Type** | Continuous 3D Points | Discrete 3D Tokens (Mol-Tokenizer) |
| **Key Innovation** | SE(3) Equivariance | Multi-molecular language modeling |
| **Best For** | Drug screening & Geometry | Synthetic chemistry & Reaction design |

