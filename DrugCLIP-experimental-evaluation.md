---
title: "DrugCLIP-experimental-evaluation"
type: Evaluation
updated: "2026-03-16"
topics:
  - Enzyme-Substrate-Alignment
papers:
  - DrugCLIP
tags:
status: in-progress
related:
---

# DrugCLIP – Experimental (Wet-Lab) Evaluation Summary

## Goal

To validate whether DrugCLIP’s virtual screening predictions translate into **real biochemical activity**, not just benchmark performance.

The key question:

> Do top-ranked molecules predicted by DrugCLIP actually bind and function in laboratory assays?

---

## Targets Tested

DrugCLIP was experimentally evaluated on three real biological targets:

- **5HT2AR** (Serotonin receptor)
- **NET** (Norepinephrine transporter)
- **TRIP12** (E3 ubiquitin ligase)

These are therapeutically relevant protein targets.

---

## Experimental Pipeline

### 1. Large-Scale Virtual Screening
- Millions of purchasable molecules were scored using DrugCLIP.
- Molecules ranked by cosine similarity between:
  - Pocket embedding
  - Molecule embedding

---

### 2. Top Enrichment Selection
- Top **1–2%** highest-scoring molecules were retained.
- This reduced millions of candidates to tens of thousands.

---

### 3. Chemical Diversity Filtering
- ~200 chemically diverse molecules selected.
- Clustering used to avoid selecting many molecules with the same scaffold.

---

### 4. Optional Docking Filter
- Docking performed on selected molecules.
- Molecules with poor docking scores removed.
- ~100 compounds per target chosen for testing.

---

### 5. Compound Purchase
- Selected compounds ordered from commercial suppliers.

---

### 6. Wet-Lab Assays
Depending on the target:

- Binding assays
- Functional cellular assays
- Enzymatic activity assays

---

## Example: 5HT2AR Results

- 78 top-ranked compounds were tested.
- 8 compounds showed measurable agonist activity.

Approximate hit rate:

> ~10%

This is significantly higher than typical random screening (<1%).

---

## NET and TRIP12 Results

- Multiple compounds showed measurable binding or functional activity.
- Confirmed that DrugCLIP predictions translate into real biochemical effects.

---

## What These Experiments Validate

The wet-lab tests demonstrate:

- Virtual screening enrichment translates to real hits.
- Embedding similarity captures meaningful interaction signals.
- The model can discover novel active scaffolds.

---

## What They Do Not Fully Prove

- Only 3 targets tested.
- Limited number of compounds (~100 per target).
- No evaluation of:
  - Toxicity
  - ADME
  - Long-term optimization
  - Clinical relevance

These experiments validate **early-stage hit discovery**, not full drug development.

---

## Overall Conclusion

DrugCLIP’s experimental validation shows:

- Strong early enrichment in real biological assays.
- Practical applicability beyond benchmark datasets.
- Evidence that embedding-based screening can discover real active compounds.

This strengthens the claim that DrugCLIP captures transferable protein–ligand interaction patterns.