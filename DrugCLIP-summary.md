---
title: "DrugCLIP Summary"
type: Summary
updated: "2026-03-16"
topics:
  - Enzyme-Substrate-Alignment
papers:
  - DrugCLIP
tags:
status: Done
related:
---

## What DrugCLIP is

**DrugCLIP** reframes virtual screening as a **retrieval problem** rather than direct affinity regression. It embeds a **protein pocket** and a **small molecule** into the same latent space, then ranks molecules for a target pocket using **cosine similarity**. This makes screening extremely fast because molecule embeddings can be precomputed offline, and screening reduces to vector similarity search. 

The model is trained in two stages:

* **Pretraining**: a new synthetic-data pipeline called **ProFSA** creates pseudo pocket–ligand pairs from protein-only structures by extracting short peptide fragments as “pseudoligands” and their surrounding regions as “pseudopockets.” This yielded about **5.5 million** pseudo pairs. 
* **Fine-tuning**: the pocket and molecule encoders are then jointly fine-tuned on about **44,000 experimentally determined protein–ligand complexes** from BioLip2, with ligand conformational augmentation from RDKit. 

## Main idea and why it matters

The central claim is that current docking and deep-learning screening methods are too slow for **genome-wide** screening, while DrugCLIP is fast enough to score **trillions** of protein–ligand pairs. The paper reports that DrugCLIP is up to **10 million times faster than docking** in their framing, and that it can screen more than **10 trillion** protein–ligand pairs in about **24 hours on 8 A100 GPUs**. 

That speed enables the paper’s biggest contribution: a public genome-scale screen called **GenomeScreenDB**, covering about **9,900 human targets**, more than **20,000 pockets**, and over **2 million candidate hit molecules** from screening **500 million compounds**. 

## Architecture in plain language

DrugCLIP has two encoders:

* a **molecule encoder**, initialized from **Uni-Mol**
* a **pocket encoder**, pretrained to align with the molecule encoder using contrastive learning on ProFSA synthetic pairs. 

During training, true pocket–ligand pairs are pulled together in embedding space and mismatched pairs are pushed apart. At inference time, a pocket is encoded once, and the model retrieves the most similar molecules from a pre-encoded library. 

## Benchmark results

On the two standard virtual screening benchmarks:

* **DUD-E**: DrugCLIP achieved **EF1% = 24.61**, outperforming Vina (**7.32**), Glide-SP (**16.18**), PLANET (**8.83**), and GNINA (**20.90**). 
* **LIT-PCBA**: DrugCLIP achieved **EF1% = 5.36**, slightly above Pafnucy (**5.32**) and above GNINA (**4.61**), BigBind (**3.55**), Glide-SP (**3.41**), and PLANET (**3.28**). 

The paper also argues that DrugCLIP generalizes better than many baselines:

* when similar ligands are removed from training, performance drops only moderately
* when homologous protein families are removed, performance still remains competitive
* it is relatively robust to pocket side-chain perturbations up to **3 Å RMSD**.  

## Speed results

The speedup is one of the strongest parts of the paper:

* LIT-PCBA can be screened in **38 seconds** in sequential mode on an A100 GPU
* the equivalent amount of work can drop to around **0.023 seconds** in a heavily parallelized setting
* by contrast, the paper reports about **3 days** for Glide-SP, **22 hours** for Uni-Dock, and **3 hours** for PLANET on that benchmark framing. 

The reason is that most methods scale like **targets × molecules**, while DrugCLIP mainly pays for **encoding targets + encoding molecules**, then performs fast similarity ranking. 

## Wet-lab validation

The paper does not only benchmark computationally; it also includes several experimental validations.

### 1) 5HT2AR

DrugCLIP screened **78 compounds** and found **8 positives** in a calcium flux assay. Of those, **6** had **Ki < 10 μM**, and all 6 achieved **β-arrestin2 NanoBiT EC50 < 1 μM**. The best hit, **V008-4481**, had **Ki = 21.0 nM** and **EC50 = 60.3 nM**.  

### 2) NET

For norepinephrine transporter, they tested **100 compounds** and found a **15% hit rate**, with **12 compounds** outperforming bupropion in the assay. Two compounds were especially strong:

* **0086-0043**: **IC50 = 1.14 μM**
* **Y510-9709**: **IC50 = 0.31 μM**

Both were structurally validated by **cryo-EM**, which is a strong point in the paper because it shows the model found chemically meaningful binders, not just assay artifacts. 

### 3) TRIP12

This is one of the most interesting case studies because TRIP12 lacked reported holo structures and known small-molecule binders. Using an AlphaFold-predicted structure plus their pocket-refinement module, they tested **57 compounds** and got a **17.5% hit rate** in SPR, with **10 compounds** below **50 μM Kd**. The top two hits were:

* **E599-0223**: **Kd = 10.8 μM**
* **G935-3912**: **Kd = 11.9 μM**

Both also inhibited TRIP12 ubiquitination activity and did **not** inhibit the upstream E1/E2 enzymes in the control assays shown in the supplement.  

## GenPack: the AlphaFold/apo-structure helper

A major practical issue is that AlphaFold or apo structures often have poorly localized pockets. To handle this, the authors introduce **GenPack**, a generative refinement pipeline:

1. detect an initial pocket with Fpocket
2. remove side chains
3. generate molecules conditioned on the backbone
4. repack/refine the pocket around those generated molecules. 

This improved virtual screening on difficult structures:

* on a 27-target DUD-E subset, **AF2-Fpocket EF1%** improved from **18.96** to **24.14** with GenPack
* **apo-Fpocket EF1%** improved from **11.56** to **20.43**. 

It also improved redocking success on 96 AlphaFold2 targets:

* **RMSD < 2 Å** improved from **19.10%** to **38.71%**. 

An important nuance from the supplement: the authors found that GenPack’s gains seem to come more from **better pocket localization** than from making side chains more “holo-like.” They report little correlation between side-chain RMSD and screening or docking performance, while pocket IoU correlates with screening quality. 

## What the supplement adds

The supplement strengthens three parts of the story.

First, it shows that **ProFSA pretraining** gives a better pocket encoder than the original Uni-Mol pocket model on several tasks:

* better pocket druggability prediction
* better zero-shot and fine-tuned pocket matching
* stronger ligand-binding affinity prediction in strict sequence-split settings. 

Second, it provides the detailed benchmark tables behind the headline results on DUD-E, LIT-PCBA, AlphaFold/apo structures, and redocking.  

Third, it shows the full wet-lab support figures for 5HT2AR, NET, and TRIP12, including dose-response curves, cryo-EM processing figures, SPR sensorgrams, and enzyme-selectivity controls. 

## Strengths of the work

The strongest aspects are:

* **A very scalable formulation** of screening as dense retrieval rather than pairwise docking. 
* **Strong benchmark performance** with a very favorable speed/accuracy tradeoff. 
* **Real experimental validation**, including cryo-EM for NET and biochemical validation for TRIP12. 
* **Practical compatibility with AlphaFold structures**, which matters a lot for under-characterized targets. 

## Limitations and caveats

The paper is impressive, but a few caveats stand out.

DrugCLIP is primarily an **early retrieval/ranking system**. It is excellent at narrowing a huge library, but the authors still use **clustering, docking, and human review** before experimental testing. So it is not a full replacement for downstream structure-based triage. 

Also, benchmark success is reported mainly in **EF1%**, which is valuable for early enrichment, but it does not directly tell you whether top hits will always be chemically novel, selective, or potent enough for lead optimization. The supplement partly addresses novelty by showing scaffold and family-removal tests, but real-world medicinal chemistry remains harder than benchmark screening. 

Finally, GenPack helps with AlphaFold and apo structures, but the authors’ own analysis suggests it does **not** truly repair side-chain accuracy in a conventional structural sense; it mainly improves **pocket localization**. 

## Bottom line

**DrugCLIP’s core contribution is not just a new model, but a new operating point for virtual screening:** fast enough to search the human proteome at enormous scale, while still accurate enough to produce real wet-lab hits. The main paper introduces the retrieval-based framework, ProFSA pretraining, GenPack for AlphaFold/apo pockets, and GenomeScreenDB; the supplement provides the detailed evidence that these components genuinely improve screening and pocket modeling.  

If useful, I can also turn this into:

1. a **1-page markdown summary**
2. a **slide-style summary**
3. a **comparison of DrugCLIP vs Uni-Mol / PLANET / docking**
