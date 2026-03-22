---
title: "ReactZyme - Summary"
type: paper-summary
updated: "2026-03-16"
topics:
  - Enzyme-Substrate-Alignment
papers:
  - ReactZyme
tags:
status: open
related:
---

# 🧬 ReactZyme — Explanation & Summary

**ReactZyme** is a machine learning framework designed for **enzyme–reaction prediction and retrieval**.
Its core idea is to learn a **joint representation space** where:

* Enzymes (proteins)
* Chemical reactions (substrates → products)

are embedded in a way that matching enzyme–reaction pairs are close to each other.

It is conceptually similar to contrastive models like **CLIP**, but applied to enzymology.

---

# 🔬 What Problem Does ReactZyme Solve?

In biology and biotechnology, we often want to answer:

* Given a reaction → *which enzyme can catalyze it?*
* Given an enzyme → *what reaction does it perform?*
* Can we retrieve candidate enzymes for a new synthetic reaction?

Traditional approaches rely on:

* EC numbers
* Sequence similarity (BLAST)
* Manual curation

ReactZyme instead learns this mapping directly from data.

---

# 🧠 Core Idea

ReactZyme builds **two encoders**:

### 1️⃣ Enzyme Encoder

Typically based on pretrained protein language models such as:

* ESM-family models (e.g., ESM2)

The enzyme sequence is converted into a fixed-length embedding.

---

### 2️⃣ Reaction Encoder

Reactions are represented by encoding:

* Substrates
* Products

using molecular encoders (often GNNs or Transformer-based models).

The reaction embedding captures the **chemical transformation pattern**.

---

### 3️⃣ Contrastive Training

ReactZyme is trained using a **contrastive loss**:

* True enzyme–reaction pairs → embeddings pulled together
* Incorrect pairs → pushed apart

So in embedding space:

```
Correct enzyme ↔ correct reaction → high cosine similarity
Wrong enzyme ↔ reaction → low similarity
```

---

# 🧪 How Reactions Are Represented

Reactions are modeled as a transformation:

```
Reaction = f(substrates, products)
```

Often done by:

* Encoding substrates separately
* Encoding products separately
* Combining them (difference, concatenation, learned fusion)

This allows the model to represent **what changes chemically**.

---

# 📊 What ReactZyme Can Do

### ✔ Enzyme Retrieval

Given a reaction → retrieve candidate enzymes ranked by similarity.

### ✔ Reaction Prediction

Given an enzyme → retrieve likely reactions.

### ✔ Generalization

It can sometimes retrieve enzymes for reactions that:

* Are not identical to training examples
* Share mechanistic similarity

---

# 🧬 Why This Is Interesting (For You Especially 😉)

Since you’re working on co-embedding enzymes and molecules (DrugCLIP-style thinking):

ReactZyme is:

* A CLIP-like framework
* Focused on reaction-level alignment (not just binding)
* Useful for metabolic pathway completion
* Potentially complementary to CLSS-style structure-aware embeddings

It’s closer to **function alignment** than binding affinity modeling.

---

# 📌 Key Differences vs DrugCLIP

| ReactZyme                     | DrugCLIP                      |
| ----------------------------- | ----------------------------- |
| Aligns enzyme ↔ reaction      | Aligns protein ↔ ligand       |
| Focus on catalysis            | Focus on binding              |
| Reaction-level representation | Molecule-level representation |
| Functional retrieval          | Virtual screening             |

---

# 🧾 Short Summary (TL;DR)

> ReactZyme is a contrastive learning framework that co-embeds enzymes and chemical reactions into a shared space, enabling retrieval of enzymes for reactions (and vice versa) by cosine similarity. It uses pretrained protein language models for enzymes and molecular encoders for reactions, trained with a CLIP-style objective.

---

