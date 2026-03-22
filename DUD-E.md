---
title: "DUD-E"
type: Summary
updated: "2026-03-16"
topics:
  - Enzyme-Substrate-Alignment
papers:
tags:
status: 
related:
---

# DUD-E (Directory of Useful Decoys: Enhanced)

## Overview

**DUD-E** is a widely used benchmark dataset for evaluating virtual screening methods in drug discovery.

It tests whether computational models can correctly rank:
- **Active ligands** (experimentally validated binders)
- Above **decoys** (property-matched, assumed non-binders)

Evaluation is performed **target-by-target**.

---

## Dataset Composition

- **102 protein targets**
- **22,886 active ligands**
- ~**50 decoys per active**
- ~**1.1 million decoys total**
- ~**1.17 million molecules overall**

Each target includes:
- A 3D protein structure (binding pocket)
- A set of experimentally confirmed actives
- A much larger set of decoys

---

## How DUD-E Was Created (Short Explanation)

1. **Target Selection**
   - Proteins with known 3D crystal structures were selected.
   - Each protein has experimentally validated ligands.

2. **Active Collection**
   - Active ligands were gathered from experimental databases.
   - Each active has measured binding affinity (e.g., IC50, Ki).

3. **Decoy Generation**
   For each active ligand:
   - ~50 decoys were selected from large chemical libraries (e.g., ZINC).
   - Decoys:
     - Match key physicochemical properties  
       (molecular weight, logP, H-bond donors/acceptors, etc.)
     - Are structurally dissimilar (low fingerprint similarity)
     - Are assumed inactive (not experimentally validated)

This creates a highly imbalanced dataset that simulates a screening scenario.

---

## Evaluation Procedure

For each protein target:

1. Encode the protein pocket.
2. Score all molecules (actives + decoys).
3. Rank molecules by predicted binding score.
4. Measure how many actives appear near the top of the ranked list.

---

## Main Metric: EF1%

**Enrichment Factor at 1% (EF1%)**

EF1% measures how enriched the top 1% of ranked molecules is with true actives compared to random selection.

### Step-by-Step Intuition

Assume a target has:

- **N total molecules**
- **A total actives**

The natural active rate is:

A / N

### 1️⃣ Expected actives by random

If we select the top 1% randomly:

Number selected = 0.01 × N

Expected actives by random:

0.01 × N × (A / N) = 0.01 × A

So random selection would recover **1% of the actives**.

---

### 2️⃣ Model performance

Let:

- Actives found in top 1% = A_top

---

### 3️⃣ EF1% formula

\[
EF1\% = \frac{A_{top}}{0.01 \times A}
\]

Which can also be interpreted as:

\[
EF1\% = \frac{\text{Actives found by model}}{\text{Actives expected by random}}
\]

---

### Interpretation

- **EF1% = 1** → random performance
- **EF1% > 1** → better than random
- **EF1% = 20** → 20× more actives in top 1% than random

EF1% emphasizes **early retrieval**, which is critical in real drug discovery, where only a small fraction of compounds can be experimentally tested.

---

## Strengths

- Large-scale and standardized
- Enables fair comparison between models
- Focuses on early enrichment (realistic screening constraint)

---

## Limitations

- Decoys are artificially generated
- Contains dataset biases
- Overestimates real-world performance
- Does not measure true hit rate, affinity prediction accuracy, or downstream drug development success

---

## Takeaway

DUD-E is:

- A useful and widely adopted benchmark
- Strong for comparing ranking-based virtual screening models

But:

> It is a semi-synthetic and optimistic approximation of real-life drug discovery performance.