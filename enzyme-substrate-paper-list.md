---
title: "Enzyme-Substrate Paper List"
type: overview
updated: "2026-03-20"
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
