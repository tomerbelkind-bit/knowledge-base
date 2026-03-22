---
title: "Papers comaprison"
type: comparison
updated: "2026-03-16"
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

# 🧬 1️⃣ ReactZyme

**Goal:**
Align **enzymes ↔ chemical reactions**

**Modality Pairing:**

* Enzyme sequence embedding (ESM-like)
* Reaction embedding (substrates + products)

**Core Idea:**
CLIP-style contrastive training between enzyme and reaction embeddings.

**Focus:**
Functional catalysis (what reaction does this enzyme perform?)

**Representation Level:**

* Enzyme: sequence
* Reaction: transformation-level representation

**Strengths:**

* Direct reaction-level supervision
* Functional retrieval
* Good for pathway completion

**Limitation:**

* No explicit structural reasoning
* Doesn’t model physical binding or geometry

---

# 🧬 2️⃣ CLIPZyme

**Goal:**
Reaction-conditioned enzyme retrieval

**Modality Pairing:**

* Enzyme (sequence + predicted structure)
* Reaction representation

**Core Idea:**
CLIP-like co-embedding with stronger structural conditioning.

**Focus:**
Find enzymes that catalyze a reaction (especially for biocatalysis).

**What’s Special:**

* Incorporates structure (AlphaFold predictions)
* More geometry-aware than ReactZyme

**Strengths:**

* Better generalization
* Structure-aware functional prediction

**Limitation:**

* Still no explicit modeling of active-site complementarity
* Mostly global embeddings

---

# 🧬 3️⃣ EnzymeCAGE

**Goal:**
Geometric foundation model for enzyme retrieval

**Modality Pairing:**

* Enzyme structure
* Reaction representation
* Evolutionary features

**Core Idea:**
Use geometric deep learning to encode structural features explicitly.

**Focus:**
Structure-driven enzyme discovery

**What’s Special:**

* 3D geometric reasoning
* Evolutionary insights integrated
* Foundation-model framing

**Strengths:**

* Explicit spatial modeling
* Better inductive bias for catalysis

**Limitation:**

* Computationally heavier
* Needs structural data

---

# 🧬 4️⃣ Dual-Encoder Contrastive Learning Accelerates Enzyme Discovery

(This is conceptually closer to ReactZyme but more general.)

**Goal:**
Use dual encoders to accelerate enzyme discovery.

**Modality Pairing:**

* Enzyme embeddings
* Reaction or substrate embeddings

**Core Idea:**
Contrastive learning over known enzyme–reaction pairs.

**Focus:**
Scalable enzyme search

**Strengths:**

* Simple
* Scalable
* Retrieval-based

**Limitation:**

* Often sequence-only
* Less mechanistic modeling

Think of it as the “clean engineering baseline” version.

---

# 🧬 5️⃣ DrugCLIP

**Goal:**
Protein–ligand interaction prediction

**Modality Pairing:**

* Protein pocket embedding
* Ligand embedding

**Core Idea:**
CLIP-style alignment for binding affinity / screening.

**Focus:**
Binding, not catalysis.

**Strengths:**

* Good for virtual screening
* Evaluated on DUD-E, LIT-PCBA
* Strong cross-target generalization

**Limitation:**

* Doesn’t model chemical transformation
* Binding ≠ catalysis

---

# 🔍 High-Level Comparison

| Model        | Aligns               | Focus     | Structure Used?     | Task Type                   |
| ------------ | -------------------- | --------- | ------------------- | --------------------------- |
| ReactZyme    | Enzyme ↔ Reaction    | Catalysis | ❌ (sequence mostly) | Functional retrieval        |
| CLIPZyme     | Enzyme ↔ Reaction    | Catalysis | ✅ (AlphaFold)       | Reaction-conditioned search |
| EnzymeCAGE   | Structure ↔ Reaction | Catalysis | ✅ (3D geometric)    | Foundation model retrieval  |
| Dual-Encoder | Enzyme ↔ Reaction    | Catalysis | Often ❌             | Scalable retrieval          |
| DrugCLIP     | Protein ↔ Ligand     | Binding   | ✅ (pocket)          | Virtual screening           |

---

# 🧠 Conceptual Difference: Two Families

### 🟢 Reaction-Level Models

* ReactZyme
* CLIPZyme
* EnzymeCAGE
* Dual-Encoder enzyme papers

They model:

```
Enzyme → Chemical transformation
```

### 🔵 Binding-Level Models

* DrugCLIP

They model:

```
Protein ↔ Ligand binding
```

Catalysis requires:

1. Binding
2. Proper orientation
3. Transition-state stabilization
4. Chemistry

DrugCLIP models only (1).

The others aim closer to (4).

---

# 🧪 From Your Perspective (Co-Embedding Thinking)

You raised earlier a very sharp point:

> What if instead of pulling embeddings close, we model complementarity?

Here’s where they stand:

* All five use **similarity-based alignment**
* None explicitly model charge complementarity or geometric complementarity
* None use a learned “interaction head” that scores compatibility explicitly

They rely on:

```
cosine_similarity(embedding_A, embedding_B)
```

That works well when both embeddings represent the *same concept*.

But enzyme–reaction and protein–ligand are **complementary systems**, not identical objects.

Your idea of:

```
Score = InteractionHead(enzyme_embedding, ligand_embedding)
```

is closer to:

* classical docking
* energy-based modeling
* learned scoring functions

And could outperform pure contrastive alignment.

---

# 🧬 Most Advanced in Structural Bias

From least structure-aware → most:

```
Dual-Encoder
ReactZyme
CLIPZyme
EnzymeCAGE
DrugCLIP (for binding tasks)
```

---

# 🧾 One-Sentence Summaries

* **ReactZyme:** CLIP for enzyme–reaction alignment.
* **CLIPZyme:** Structurally enhanced reaction-conditioned enzyme retrieval.
* **EnzymeCAGE:** Geometric foundation model for enzyme discovery.
* **Dual-Encoder enzyme paper:** Scalable contrastive enzyme–reaction retrieval.
* **DrugCLIP:** CLIP for protein–ligand binding.

---

