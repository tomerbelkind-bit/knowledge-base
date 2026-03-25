---
title: "Enzyme-Substrate Paper List"
type: overview
updated: "2026-03-24"
topics:
  - Enzyme-Substrate-Alignment
papers:
tags:
status: 
related:
---

# Enzyme-Substrate Paper List

### RC-GNN (Reaction-Center Graph Neural Network) (2025/2026)

### ChemHGNN (Hierarchical Hypergraph Neural Network) (2026)

### FGW-CLIP (2025)

**Focus**: Improving the "alignment" math.

**Contribution**: Traditional CLIP models only align between domains (Enzyme $\leftrightarrow$ Reaction). FGW-CLIP uses Fused Gromov-Wasserstein distance to also align within domains (ensuring similar enzymes stay near each other). This preserves the internal structural hierarchy of chemical space better than standard contrastive loss.

### VenusRXN (2026)

**Focus**: Reaction-conditioned enzyme discovery.

**Contribution**: Integrates a Graph Transformer (Mol-Graphormer) with a Protein Language Model. It is unique because it encodes the "Reaction Center"—the specific atoms undergoing bond changes—giving it higher sensitivity to the actual chemistry happening in the pocket.

### CLEAN (Contrastive Learning for Enzyme Annotation)

**Focus**: Enzyme Commission (EC) number prediction.

**Contribution**: While not a direct "alignment" with substrates, it pioneered the use of contrastive learning to separate enzymes into functional clusters, which many alignment models (like CLIPZyme) now use as a pre-trained backbone.


## Global embedding alignment (CLIP-style)

| Paper                             | Year | Core idea                      | What is aligned               | Alignment type       | Notes                     |
| --------------------------------- | ---- | ------------------------------ | ----------------------------- | -------------------- | ------------------------- |
| **DeepDTA**                       | 2018 | CNN on protein + SMILES        | protein ↔ ligand              | global               | Early baseline            |
| **DeepAffinity**                  | 2018 | Sequence + SMILES affinity     | protein ↔ ligand              | global               | Similar to DeepDTA        |
| **GraphDTA**                      | 2020 | GNN for molecules              | protein ↔ ligand              | global               | Adds molecular graph      |
| **DrugCLIP**                      | 2023 | Contrastive protein–ligand     | protein ↔ ligand              | global               | Uses pseudo pockets       |
| **CLIPZyme**                      | 2023 | Contrastive enzyme–reaction    | enzyme ↔ reaction             | global               | Key enzyme–reaction paper |
| **Dual-encoder enzyme discovery** | 2024 | CLIP-style retrieval           | enzyme ↔ reaction             | global               | Clean baseline            |
| **EnzyCLIP**                      | 2025 | Dual encoder + cross-attention | enzyme ↔ substrate            | global + light local | Adds kinetics supervision |
| **FGW-CLIP**                      | 2025 | Optimal transport alignment    | enzyme graph ↔ reaction graph | global + structural  | Geometry-aware alignment  |


| Paper                             | Year | Sequence      | Structure | Reaction           | Pocket    | Notes                             |
| --------------------------------- | ---- | ------------- | --------- | ------------------ | --------- | --------------------------------- |
| **DeepDTA**                       | 2018 | ✅             | ❌         | ❌                  | ❌         | Protein sequence + SMILES         |
| **DeepAffinity**                  | 2018 | ✅             | ❌         | ❌                  | ❌         | Similar to DeepDTA                |
| **GraphDTA**                      | 2020 | ✅             | ❌         | ❌                  | ❌         | Molecule graph only               |
| **DrugCLIP**                      | 2023 | ✅             | ❌         | ❌                  | ⚠️ pseudo | Uses inferred pockets             |
| **CLIPZyme**                      | 2023 | ✅ (ESM)       | ❌         | ✅                  | ❌         | Reaction fingerprints (e.g. DRFP) |
| **Dual-encoder enzyme discovery** | 2024 | ✅             | ❌         | ✅                  | ❌         | Clean CLIP-style                  |
| **EnzyCLIP**                      | 2025 | ✅             | ❌         | ❌ (substrate only) | ❌         | Molecule ≠ reaction               |
| **FGW-CLIP**                      | 2025 | ⚠️ (optional) | ✅         | ✅                  | ❌         | Graph alignment (structure-heavy) |


## Structure-aware / graph alignment

| Paper          | Year | Core idea                     | What is aligned            | Alignment type           | Notes                       |
| -------------- | ---- | ----------------------------- | -------------------------- | ------------------------ | --------------------------- |
| **SchNet**     | 2017 | Continuous-filter GNN         | atoms                      | structural               | Used for molecules          |
| **EGNN**       | 2021 | Equivariant GNN               | atoms / residues           | structural               | Geometry-preserving         |
| **GVP-GNN**    | 2021 | Vector + scalar features      | residues                   | structural               | Protein representation      |
| **ReactZyme**  | 2024 | Matching matrix + attention   | enzyme ↔ reaction          | structural scoring       | Retrieval benchmark         |
| **EnzymeCAGE** | 2024 | Multi-modal interaction model | enzyme ↔ substrate/product | structural + interaction | Combines GVP + SchNet       |
| **FGW-CLIP**   | 2025 | Graph optimal transport       | enzyme ↔ reaction          | structural               | Strong structural alignment |


| Paper          | Year | Sequence      | Structure    | Reaction | Pocket       | Notes                      |
| -------------- | ---- | ------------- | ------------ | -------- | ------------ | -------------------------- |
| **SchNet**     | 2017 | ❌             | ✅ (molecule) | ❌        | ❌            | Molecular structure        |
| **EGNN**       | 2021 | ❌             | ✅            | ❌        | ❌            | Generic geometry model     |
| **GVP-GNN**    | 2021 | ⚠️ (features) | ✅            | ❌        | ❌            | Protein structure          |
| **ReactZyme**  | 2024 | ✅             | ✅ (via ESM2) | ✅        | ❌            | Reaction + sequence        |
| **EnzymeCAGE** | 2024 | ❌             | ✅            | ✅        | ❌ (implicit) | Uses full enzyme structure |
| **FGW-CLIP**   | 2025 | ⚠️            | ✅            | ✅        | ❌            | Structural alignment       |


## Local interaction / mechanistic alignment

| Paper                                  | Year      | Core idea                      | What is aligned      | Alignment type | Notes                        |
| -------------------------------------- | --------- | ------------------------------ | -------------------- | -------------- | ---------------------------- |
| **AlphaFold2**                         | 2021      | Protein structure prediction   | residues             | structural     | Enables downstream alignment |
| **Docking pipelines (AutoDock, etc.)** | 2010s     | Physics-based binding          | enzyme ↔ ligand pose | physical       | Not ML but gold baseline     |
| **OmniESI**                            | 2025      | Two-stage interaction modeling | enzyme ↔ substrate   | local + global | Catalysis-aware              |
| **VenusRXN (and similar)**             | ~2025     | Cross-attention interaction    | residue ↔ atom       | local          | Fine-grained alignment       |
| **Pocket-based GNN models**            | 2022–2025 | Active-site modeling           | pocket ↔ ligand      | local          | Increasing trend             |


| Paper                       | Year      | Sequence | Structure     | Reaction | Pocket        | Notes                       |
| --------------------------- | --------- | -------- | ------------- | -------- | ------------- | --------------------------- |
| **AlphaFold2**              | 2021      | ✅        | ✅ (predicted) | ❌        | ❌             | Enables downstream models   |
| **Docking (AutoDock etc.)** | 2010s     | ❌        | ✅             | ❌        | ✅             | Explicit binding simulation |
| **OmniESI**                 | 2025      | ✅        | ✅             | ❌        | ⚠️ (implicit) | Learns interaction patterns |
| **VenusRXN-type**           | ~2025     | ✅        | ✅             | ⚠️       | ✅             | Residue–atom alignment      |
| **Pocket GNN models**       | 2022–2025 | ❌        | ✅             | ❌        | ✅             | Explicit active-site focus  |


## Function-space alignment (enzyme-only)

| Paper                | Year      | Core idea                 | What is aligned   | Alignment type | Notes              |
| -------------------- | --------- | ------------------------- | ----------------- | -------------- | ------------------ |
| **PRIAM / DeepEC**   | 2010–2015 | EC prediction             | enzyme ↔ EC class | functional     | Pre-deep-learning  |
| **ProtBERT / ESM1b** | 2020–2021 | Protein LM embeddings     | enzyme ↔ enzyme   | intra-domain   | General embeddings |
| **CLEAN**            | 2023      | Contrastive EC prediction | enzyme ↔ enzyme   | intra-domain   | No substrate info  |


| Paper              | Year      | Sequence | Structure | Reaction | Pocket | Notes              |
| ------------------ | --------- | -------- | --------- | -------- | ------ | ------------------ |
| **CLEAN**          | 2023      | ✅        | ❌         | ❌        | ❌      | EC prediction only |
| **ESM / ProtBERT** | 2020–2021 | ✅        | ❌         | ❌        | ❌      | General embeddings |
| **DeepEC / PRIAM** | 2010s     | ✅        | ❌         | ❌        | ❌      | Classical methods  |



## Generative alignment (design)

| Paper                        | Year      | Core idea                      | What is aligned      | Alignment type     | Notes                 |
| ---------------------------- | --------- | ------------------------------ | -------------------- | ------------------ | --------------------- |
| **Protein diffusion models** | 2023–2024 | Structure generation           | sequence ↔ structure | implicit           | Foundation for design |
| **EnzyGen**                  | 2024      | Generate enzyme from substrate | substrate → enzyme   | implicit alignment | Uses interaction loss |
| **EnzyControl**              | 2025      | Controlled enzyme design       | substrate ↔ enzyme   | implicit           | Backbone generation   |


| Paper                        | Year      | Sequence | Structure | Reaction | Pocket        | Notes                    |
| ---------------------------- | --------- | -------- | --------- | -------- | ------------- | ------------------------ |
| **EnzyGen**                  | 2024      | ❌        | ✅         | ❌        | ⚠️ (implicit) | Conditioned on substrate |
| **EnzyControl**              | 2025      | ❌        | ✅         | ❌        | ⚠️            | Backbone generation      |
| **Diffusion protein design** | 2023–2024 | ❌        | ✅         | ❌        | ⚠️            | General protein design   |


## Evolution of the field (very important insight)

Now with dates, you can clearly see the progression:

**2017–2020: Representation learning**
- SchNet, DeepDTA, GraphDTA
👉 “Can we encode molecules and proteins?”

**2021: Geometry + structure revolution**
- AlphaFold2, EGNN, GVP
👉 “We now understand 3D structure”

**2023: Contrastive alignment begins**
- CLIPZyme, DrugCLIP, CLEAN
👉 “Let’s align modalities”

**2024: Reaction-aware alignment**
- EnzymeCAGE, ReactZyme, dual-encoder
👉 “Let’s include chemistry explicitly”

**2025+: Structured & multi-level alignment**
- FGW-CLIP, EnzyCLIP, OmniESI
👉 “Let’s align structure + interactions + reactions”

## What stands out (important for your thinking)

CLEAN (2023) → alignment without substrate
CLIPZyme (2023) → first strong enzyme–reaction alignment
EnzymeCAGE (2024) → introduces reaction + structure fusion
FGW-CLIP (2025) → introduces geometry-aware alignment

👉 The trend is clearly:

from vector similarity → structure → interaction → chemistry-aware alignment

## Most models choose ONE dominant modality
Sequence-only → CLEAN, CLIPZyme
Structure-only → EnzymeCAGE, GVP
Reaction-only → DRFP-based models

👉 Very few combine all.

## Reaction vs Structure is a major divide

| Type                         | Strength           | Weakness                   |
| ---------------------------- | ------------------ | -------------------------- |
| Reaction-based (CLIPZyme)    | captures chemistry | ignores geometry           |
| Structure-based (EnzymeCAGE) | captures binding   | weak chemistry abstraction |

👉 This is a core gap in the field


## Pocket usage is surprisingly rare

Explicit pocket: docking, VenusRXN-like
Implicit (attention learns it): EnzymeCAGE, OmniESI
Not used: CLIP-style model

👉 Meaning:

most models don’t explicitly localize the catalytic site

## Only a few models use ALL key inputs

| Paper             | Uses sequence | structure | reaction | pocket |
| ----------------- | ------------- | --------- | -------- | ------ |
| **FGW-CLIP**      | ⚠️            | ✅         | ✅        | ❌      |
| **EnzymeCAGE**    | ❌             | ✅         | ✅        | ❌      |
| **VenusRXN-type** | ✅             | ✅         | ⚠️       | ✅      |

👉 And none of them use:

✅ sequence + structure + reaction + pocket together

 
## Big research gap (very relevant to you)

The “ideal” model would take:

sequence → evolutionary signal
structure → geometry
reaction → chemistry
pocket → locality

👉 And align them at two levels:

global (CLIP-style)
local (residue ↔ atom)


