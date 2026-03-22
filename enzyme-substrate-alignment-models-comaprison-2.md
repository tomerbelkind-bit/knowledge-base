---
title: "Enzyme-Substrate Alignment Models - Comparison (2)"
type: comparison
updated: "2026-03-18"
topics:
  - Enzyme-Substrate-Alignment
papers:
  - CLIPZyme
  - DrugCLIP
  - Dual-encoder
  - EnzymeCAGE
  - ReactZyme
tags:
status: open
related:
---

# Functional Proteomics - Comparison

## Comparative Breakdown

| Model | Primary Architecture | Chemistry Representation | Key Advantage |
| :--- | :--- | :--- | :--- |
| **Horizyn-1** | Dual-Encoder Contrastive | **Reaction Fingerprints** (ECFP6/DRFP) | Massive scale; excels at **retrieval** from millions of proteins. |
| **CLIPZyme** | Dual-Encoder Contrastive | **Graphs** (Molecular Graphs) | Better at understanding **geometric/structural** details of the substrate. |
| **EnzymeCAGE** | Generative/Graph-based | **Site-specific** (Active sites) | Focuses on the **local environment** where the reaction actually happens. |
| **ReactZyme** | Contrastive Learning | Multi-modal (Text + Graph) | Integrates **natural language** descriptions of reactions. |

---

## 1. Horizyn-1 vs. CLIPZyme
**CLIPZyme** was one of the first to apply the "CLIP" (Contrastive Language-Image Pre-training) logic to enzymes.
* **The Difference:** CLIPZyme treats molecules as **graphs** (nodes and edges). This is computationally expensive but captures the 3D-like connectivity well. 
* **The Horizyn-1 Edge:** Horizyn-1 uses **Reaction Fingerprints**. While fingerprints are "flatter" than graphs, they are much faster to process. This allowed the Horizyn-1 team to train on **8.9 million pairs**, whereas CLIPZyme used a much smaller dataset (~100k pairs). Horizyn-1 is essentially the "Big Data" version of this approach.



---

## 2. Horizyn-1 vs. EnzymeCAGE
**EnzymeCAGE** is more of a "microscope" than a search engine.
* **The Difference:** EnzymeCAGE focuses on **Catalytic Anchor Group Enumeration (CAGE)**. It looks specifically at the amino acids that touch the substrate (the active site). 
* **The Horizyn-1 Edge:** Horizyn-1 looks at the **entire protein sequence**. While EnzymeCAGE is fantastic for understanding *why* an enzyme works or how to engineer a better one, Horizyn-1 is better at *finding* a completely new protein in a database that might have a totally different active site structure than what we've seen before.

---

## 3. Horizyn-1 vs. ReactZyme
**ReactZyme** attempts to bridge the gap between human language and chemistry.
* **The Difference:** ReactZyme often incorporates text-based descriptions of reactions. It’s designed to handle "multi-modal" queries where you might describe a reaction in words.
* **The Horizyn-1 Edge:** Horizyn-1 is purely focused on the **chemical transformation math**. By stripping away the ambiguity of human language and focusing on the delta between Reactant $A$ and Product $B$, it achieves higher precision in "orphan" reaction discovery where no text descriptions even exist yet.

---

## Summary of the "Vibe"
* **CLIPZyme:** The structural pioneer. Great for small-scale, high-detail substrate matching.
* **EnzymeCAGE:** The surgeon. Best for looking at active sites and engineering specific mutations.
* **ReactZyme:** The generalist. Useful for linking literature and chemistry.
* **Horizyn-1:** The powerhouse. Built for **speed and scale** to scan the entire tree of life for a specific reaction.
