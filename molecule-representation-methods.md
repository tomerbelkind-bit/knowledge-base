---
title: "Molecule Representation Methods"
type: overview
updated: "2026-03-18"
topics:
  - Enzyme-Substrate-Alignment
papers:
tags:
status: 
related:
---

# Molecule Representation Methods

In the world of AI-driven chemistry, how you "translate" a molecule into a computer-readable format (a representation) determines what the model can learn.
Here is a breakdown of the primary methods to represent molecules and enzymes, along with which of the papers you mentioned utilize them.

---

## 1. String-Based Representations
These are the simplest forms, treating a molecule like a sentence.
* **SMILES (Simplified Molecular Input Line Entry System):** Represents the molecule's structure as a single line of text (e.g., Ethanol is `CCO`).
* **SELFIES:** A newer string format designed to be "100% robust," meaning every possible string represents a valid molecule (unlike SMILES, which can have "typos" that break the chemistry).
* **Usage:** **ReactZyme** uses SMILES as part of its multi-modal input.

---

## 2. Feature-Based (Fingerprints)
Instead of the whole structure, these capture "snapshots" of local chemical environments.
* **ECFP (Extended Connectivity Fingerprints):** Also called **Morgan Fingerprints**. It looks at each atom and its neighbors in increasing circles, hashing these patterns into a bit-string (0s and 1s).
* **DRFP (Differential Reaction Fingerprints):** Specifically designed for reactions; it calculates the "mathematical difference" between the reactant fingerprint and the product fingerprint.
* **Usage:** **Horizyn-1** relies heavily on a hybrid of ECFP6 and DRFP. It chose these for their extreme speed and ability to scale to millions of reactions.



---

## 3. Graph-Based Representations (2D)
These treat atoms as nodes and bonds as edges, processed by **Graph Neural Networks (GNNs)**.
* **How it works:** The model "passes messages" between connected atoms to learn how the local chemistry affects the whole molecule.
* **Usage:** * **CLIPZyme:** Uses Directed Message Passing Neural Networks (DMPNN) to combine substrate and product graphs into a "pseudo-transition state."
    * **ReactZyme:** Uses 2D graphs via the Uni-Mol framework to capture topological patterns.
    * **EnzymeCAGE:** Uses GNNs to represent the reaction center (the specific atoms that change).

---

## 4. Geometric & 3D Representations
These account for the actual $(x, y, z)$ coordinates of atoms, capturing bond angles and steric "bulk."
* **3D Conformations:** Predicting how a molecule folds in space.
* **Surface/Pocket Mapping:** Focusing specifically on the "cavity" (active site) of an enzyme.
* **Usage:**
    * **EnzymeCAGE:** This is its "superpower." It uses a **Geometry-Aware** architecture to focus on the 3D structure of the enzyme's catalytic pocket.
    * **ReactZyme:** Incorporates 3D conformations (Uni-Mol-3D) to understand spatial chemical environments.
    * **CLIPZyme:** Uses 3D coordinates (via EGNNs) to align the protein's physical shape with the chemical reaction.



---

## 5. Sequence-Based (Language Models)
Specifically for enzymes (proteins), these treat the amino acid chain like human language.
* **Protein Language Models (PLMs):** Models like **ESM-2** or **ProtT5** "read" millions of protein sequences to learn the rules of biological folding and function.
* **Usage:** **Horizyn-1**, **CLIPZyme**, and **ReactZyme** all use PLMs to encode the enzyme side of the equation.

---

## Summary Table: Who Uses What?

| Paper | Representation Strategy | Why they chose it? |
| :--- | :--- | :--- |
| **Horizyn-1** | **Fingerprints** (ECFP6/DRFP) + PLMs | **Scalability.** Best for searching billions of possibilities quickly. |
| **CLIPZyme** | **2D Graphs** (DMPNN) + 3D Coordinates | **Alignment.** Best for matching reaction "flow" to protein shape. |
| **EnzymeCAGE** | **3D Geometry** (Pocket-specific) | **Specificity.** Best for pinpointing exactly where the reaction happens. |
| **ReactZyme** | **Multi-modal** (Strings + Graphs + 3D) | **Comprehensiveness.** Tries to use every available "view" of the molecule. |
