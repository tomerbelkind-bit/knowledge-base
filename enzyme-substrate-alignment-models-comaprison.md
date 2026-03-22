---
title: "Enzyme-Substrate Alignment Models - Comaprison"
type: comparison
updated: "2026-03-18"
topics:
  - Enzyme-Substrate-Alignment
papers:
  - CLIPZyme
  - DrugCLIP
  - Dual-encoder
  - ReactZyme
  - EnzymeCAGE
tags:
status: open
related:
---

# Enzyme-Substrate Alignment Models - Comaprison

This comparison explores five cutting-edge models that leverage contrastive learning to bridge the gap between chemistry (reactions/molecules) and biology (enzymes/proteins). While they share the "CLIP-style" dual-encoder philosophy, they differ significantly in their scale, structural focus, and specific application.

---

## Comparative Overview

| Feature | **DrugCLIP** | **CLIPZyme** | **ReactZyme** | **Dual-Encoder (Horizyn-1)** | **EnzymeCAGE** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Pub. Date** | Oct 2023 | May 2024 | Nov 2024 | Aug 2025 | Jan 2026 |
| **Primary Goal** | Virtual Screening (Drug-Target) | Enzyme Retrieval (Reaction-Enzyme) | Benchmark for Enzyme-Reaction Prediction | Large-scale Enzyme Discovery | Functional Prediction & Retrieval |
| **Matching Method** | Contrastive Co-embedding | Reaction-Protein Alignment | Multi-modal Contrastive | Reaction-Sequence Alignment | Catalytic-Specific Geometric Alignment |

---

## Detailed Representation & Methodology

### 1. Small Molecule Representation
* **DrugCLIP:** Uses **molecular graphs** processed by GNNs. It focuses on the chemical features relevant to binding pockets.
* **CLIPZyme:** Employs **2D reaction graphs**. It treats the reaction as a transformation from reactant to product.
* **ReactZyme:** A multi-modal approach using **SMILES strings** and **2D/3D graphs** (via Uni-Mol) to provide a comprehensive view.
* **Horizyn-1:** Uses **Reaction Fingerprints** (ECFP6/DRFP). This allows for massive scalability and fast retrieval.
* **EnzymeCAGE:** Focuses on the **Reaction Center**. It extracts local geometric features of the atoms directly involved in the catalysis.



### 2. Protein Representation
* **DrugCLIP:** Focuses on the **Binding Pocket** rather than the whole sequence, using a pocket encoder to capture 3D site geometry.
* **CLIPZyme:** Uses **3D Protein Graphs** processed by an *E(n)*-equivariant GNN to understand the enzyme's physical shape.
* **ReactZyme:** Primarily relies on **Protein Language Models (PLMs)** like ESM-2 to encode the amino acid sequence.
* **Horizyn-1:** Utilizes **ProtT5**, a large-scale PLM that treats protein sequences as "text" to extract functional "semantics."
* **EnzymeCAGE:** A **Geometric Foundation Model** that uses both the global structure and the specific local geometry of the catalytic site.



### 3. Key Advantages
* **DrugCLIP:** Outstanding **zero-shot** performance; eliminates the need for explicit binding-affinity labels during training.
* **CLIPZyme:** First to use a "pseudo-transition state" approach, helping the model understand the *process* of a reaction.
* **ReactZyme:** Provides the most diverse benchmark, allowing researchers to test models across various data-split scenarios (time-based vs. similarity-based).
* **Horizyn-1:** The "Big Data" winner. Its scale (8.9M pairs) allows it to discover **"orphan" reactions** where no other model succeeds.
* **EnzymeCAGE:** **Interpretability.** It can pinpoint the exact catalytic residues responsible for a reaction, making it a tool for both discovery and engineering.

---

## Training, Evaluation, and Results

### Training Datasets
* **DrugCLIP:** Trained on **PDBbind** and **BioLip** (focus on drug-like molecules and binding sites).
* **CLIPZyme:** Trained on **EnzymeMap** (~50k balanced enzymatic reactions).
* **ReactZyme:** Uses the massive **SwissProt + Rhea** dataset (~178k pairs).
* **Horizyn-1:** The largest training set to date: **8.9 million** reaction-enzyme pairs.
* **EnzymeCAGE:** Trained on approximately **1.5 million** structural and evolutionary data points.

### Evaluation & Results
* **DrugCLIP:** Evaluated on **DUD-E** and **LIT-PCBA**. It surpassed traditional docking (like Glide) in 80% of human-judged cases.
* **CLIPZyme:** Measured using **BEDROC** and **Enrichment Factors (EF)**. Showed marked improvement over traditional EC-number prediction.
* **ReactZyme:** Benchmarked on its own dataset, showing up to **88% relative improvement** in retrieval over simpler sequence-similarity methods.
* **Horizyn-1:** Lab-validated. Successfully identified "missing" genes in *P. putida* and found enzymes for **non-natural** industrial reactions.
* **EnzymeCAGE:** Demonstrated state-of-the-art results in **orphan reaction de-orphaning** and accurately identified catalytic sites across diverse enzyme families.

---

## Summary: The Evolution of the Field
This progression shows a clear trend: moving from **drug-binding** (DrugCLIP) to **reaction-matching** (CLIPZyme), then to **massive scaling** (Horizyn-1), and finally to **geometric precision** (EnzymeCAGE). While DrugCLIP is the tool of choice for medicinal chemists, Horizyn-1 and EnzymeCAGE represent the future of industrial biocatalysis and metabolic engineering.