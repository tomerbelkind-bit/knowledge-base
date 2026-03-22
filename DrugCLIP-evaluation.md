---
title: "DrugCLIP-evaluation"
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

# DrugCLIP — Evaluation Summary

DrugCLIP is evaluated across four main dimensions:

1. Virtual screening accuracy  
2. Generalization robustness  
3. Speed and scalability  
4. Experimental (wet-lab) validation  

---

## 1. Virtual Screening Benchmarks

### DUD-E

- About 102 protein targets  
- Each target includes known active ligands and many decoys  
- The task is to rank active molecules above decoys  

Metric used: Enrichment Factor at 1 percent (EF1%)

EF1% measures how many true actives appear in the top 1 percent of ranked molecules compared to random selection.

Result:
DrugCLIP outperforms or matches major baselines such as:
- AutoDock Vina  
- Glide-SP  
- PLANET  
- GNINA  

This shows strong early enrichment performance.

---

### LIT-PCBA

- 15 protein targets  
- Real high-throughput screening data  
- More realistic and less biased than DUD-E  

Result:
DrugCLIP performs competitively with docking and other deep learning models on this harder benchmark.

---

## 2. Generalization Tests

DrugCLIP is tested under distribution shift to assess robustness.

### A. Ligand similarity removal

- Similar ligands are removed from training data  
- Performance drops moderately but remains competitive  

This suggests the model is not purely memorizing ligands.

---

### B. Protein family removal

- Homologous protein families are removed from training  
- DrugCLIP still performs well  

This demonstrates pocket-level generalization.

---

### C. Pocket perturbation

- Side chains are perturbed up to 3 angstrom RMSD  
- Performance degrades gradually  
- The model remains robust to moderate structural noise  

This supports compatibility with imperfect or AlphaFold-derived structures.

---

## 3. Speed and Scalability

Traditional docking scales roughly with:

number_of_targets multiplied by number_of_molecules

DrugCLIP scales approximately with:

number_of_targets plus number_of_molecules

Reason:
- Molecules are embedded once  
- Screening reduces to cosine similarity search  

Reported performance:

- LIT-PCBA screened in seconds on a single A100 GPU  
- Around 10 trillion protein–ligand pairs screened in about 24 hours using 8 GPUs  

This enables genome-scale screening.

---

## 4. Genome-Wide Demonstration

DrugCLIP was applied to:

- About 9,900 human proteins  
- Over 20,000 predicted pockets  
- 500 million molecules screened  

The results were released as GenomeScreenDB.

---

## 5. Experimental Validation

Predicted hits were tested experimentally.

### 5HT2AR

- 78 compounds tested  
- 8 positives  
- One nanomolar hit identified  

---

### NET (Norepinephrine Transporter)

- 100 compounds tested  
- 15 percent hit rate  
- Cryo-EM validation for top compounds  

---

### TRIP12

- AlphaFold structure used  
- 57 compounds tested  
- 17.5 percent hit rate  
- Biochemical validation confirmed activity  

These experiments demonstrate real-world utility beyond benchmarks.

---

# Overall Evaluation Summary

Benchmark enrichment: Strong  
Generalization: Good  
Structural robustness: Good  
Speed: Extremely strong  
Wet-lab validation: Successful  

---

## Important Caveats

- Focuses on early enrichment rather than full binding affinity prediction  
- Not designed for high-precision pose prediction  
- Intended as a first-pass large-scale screening engine  

---

# Positioning

DrugCLIP is evaluated as:

A fast, scalable retrieval model for structure-based virtual screening.

It is not positioned as:
- A docking replacement for precise pose modeling  
- A binding affinity regression model  
- A generative chemistry system  