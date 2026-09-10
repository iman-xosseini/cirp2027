# Label-Efficient And Explainable Defect Detection In Injection Moulding Using Self-Supervised Joint-Embedding Predictive Representations

![Status: Pre-Release (Abstract Stage)](https://img.shields.io/badge/Status-Pre--Release-orange.svg)
[![Target: CIRP CMS 2027](https://img.shields.io/badge/Target-CIRP_CMS_2027-blue.svg)](https://cms2027.com/)

<!-- [![Status: Pre-Release (Abstract Stage)](https://img.shields.io/badge/Status-Pre--Release-orange.svg)]()
[![Target: CIRP CMS 2027](https://img.shields.io/badge/Target-CIRP_CMS_2027-blue.svg)]() -->

## Abstract

In injection moulding, quality inspection is costly and defects are rare, making labeled data scarce and limiting data-driven quality monitoring. We present a self-supervised framework that learns from unlabeled production cycles, selects cycles worth inspecting, and explains its decisions in sensor terms. The data consist of injection-moulding cycles, each described by 26 sensor measurements and treated as an independent multivariate snapshot rather than part of a time series. From each cycle, we hide a subset of sensors and train the model to predict a compact latent representation of the complete cycle from the remaining measurements, learning relationships among machine variables without reconstructing raw readings. This adapts the core LeJEPA idea to discrete manufacturing using a modified architecture for multivariate sensor snapshots. Individual sensor masking is ineffective because 19 of 26 sensors have a partner correlated above 0.9, making 64\% of hidden sensors recoverable from a neighbor. Masking whole correlated groups reduces this information leakage by preventing reliance on highly correlated visible sensors. A statistical regularizer prevents collapse of the learned representations, enabling latent prediction error to rank unlabeled cycles. Across five runs on 5,232 cycles containing 1.15\% defects, inspecting the 300 highest-ranked cycles identified 8–10 defective cycles per run, versus 2–4 under random selection, for the same inspection effort. The defect classifier uses raw sensor variables directly, linking predictions to measurable process quantities and sensor-value ranges. An injection moulding expert evaluates the explanations in a blinded test with a scrambled-sensor control, and the model can be retrained quickly on a standard CPU.

## Overview
This repository contains the codebase for the upcoming research paper introducing a novel framework for anomaly detection in discrete manufacturing systems, demonstrated on industrial injection moulding processes.

Traditional generative architectures struggle with noisy industrial sensor data, and black-box models lack the transparency required for human-in-the-loop engineering. To address these gaps, this project integrates a **Latent-Euclidean Joint-Embedding Predictive Architecture (LeJEPA)** with **Active Learning** and **Explainable AI (SHAP)**. By modeling each manufacturing cycle as an independent snapshot, the system learns physical machine dynamics without reconstructing unpredictable sensor noise.

## Proposed Architecture

```text
        SELF-SUPERVISED PRE-TRAINING  
        ┌─────────────────────────────────────────┐
        │         ONE MANUFACTURING CYCLE         │
        │         26 standardized sensors         │
        └────────────────────┬────────────────────┘
                             │
        ┌────────────────────┬────────────────────┐
        │                BLOCK MASK               │
        │        9 correlated sensor blocks       │
        └────────────────────┬────────────────────┘
                  ┌──────────┴──────────┐
            context view           target view
         [ x * mask , mask ]      [ x , ones ]
                  ↓                     ↓
          ┌───────┬─────────────────────┬───────┐
          │            SHARED ENCODER           │
          │           52 → 64 → 32 → 8          │
          │           LayerNorm + GELU          │
          └───────┬─────────────────────┬───────┘
              z_context             z_target
                (8-d)                 (8-d)
                  ↓                     │
         ┌────────┬────────┐            │
         │    PREDICTOR    │            │
         │    8 → 32 → 8   │            │
         └────────┬────────┘            │
                  ↓                     │
               z_pred                   │
                  │                     │
                  └──────────┬──────────┘
        ┌────────────────────┬────────────────────┐
        │                   LOSS                  │
        │ MSE( z_target , z_pred )  +  λ · SIGReg │
        └────────────────────┬────────────────────┘
                             │
        ═════════════════════│═════════════════════
                             ↓
        DOWNSTREAM   (labels enter here)
        ┌─────────────────────────────────────────┐
        │     ACTIVE LEARNING  (model frozen)     │
        │ score = ‖z_target − z_pred‖  (10 masks) │
        │     query the top 300 of 5232 cycles    │
        └────────────────────┬────────────────────┘
                             │
        ┌────────────────────┬────────────────────┐
        │              RANDOM FOREST              │
        │    trained on the 300 raw sensor rows   │
        └────────────────────┬────────────────────┘
                             │
        ┌────────────────────┬────────────────────┐
        │                   SHAP                  │
        │    attributions in engineering units    │
        │        physical operating window        │
        └────────────────────┬────────────────────┘
                             │
        ┌────────────────────┬────────────────────┐
        │         DOMAIN EXPERT  (blinded)        │
        │plausibility · actionability · root cause│
        │         vs permuted-name control        │
        └─────────────────────────────────────────┘
```

The pipeline operates in three distinct phases:

1. **Self-Supervised Pre-Training (LeJEPA):** Utilizes an intra-cycle **correlated block-masking** strategy to prevent data leakage from highly correlated sensors. A Shared MLP Encoder (which explicitly receives both the masked data and the mask itself to avoid zero-value ambiguity) and a Predictor network learn to predict the latent representations of the full cycle based on available concurrent block readings. This training is stabilized by **Sketched Isotropic Gaussian Regularization (SIGReg)** applied to both live branches, which prevents representation collapse without needing stop-gradients or teacher networks.

2. **Active Learning Loop:** The trained, frozen networks evaluate unlabelled datasets. Cycles with the highest epistemic uncertainty—measured via the Euclidean distance between predicted and true latent states and **averaged over 10 independent mask draws** to eliminate random noise—are queried for expert labeling (e.g., the top 300 cycles), drastically reducing annotation bottlenecks.

3. **Physical-Space Explainability (XAI) & Human Validation:** A downstream classifier (Random Forest) is trained on the raw, unmasked physical inputs of the actively queried budget. SHAP (TreeExplainer) is applied directly to these physical inputs to trace anomalies back to tangible root causes (e.g., pressure or temperature variations) and define a 2D physical operating window, devoid of temporal confounding. Crucially, these explanations are then **blindly evaluated by a domain expert** for physical plausibility, actionability, and root cause against a permuted-name control to mathematically prove the explanations carry genuine sensor-specific information.

## Roadmap & Planned Evaluations

This repository is currently in the **Abstract Submission** phase. Upcoming commits will introduce the full evaluation suite to validate the methodology:

* [ ] **Active Learning Efficiency:** Benchmarking the LeJEPA latent-error querying strategy against standard Random Sampling.
* [ ] **Representation Quality (Ablation):** Comparing the LeJEPA regularized latent space against traditional Autoencoder reconstructions.
* [ ] **Qualitative XAI Validation:** Mapping SHAP attributions of True Positives to known injection molding physics.

## Repository Structure (Planned)

```text
├── data/                  # Standardized injection molding datasets
├── models/                # PyTorch definitions for LeJEPA Encoder and Predictor
├── notebooks/             # Jupyter notebooks for pipeline debugging and visualization
├── src/                   # Core Python scripts (Pre-training, Active Learning, SHAP)
└── README.md              # Project documentation

```

## Contact

### Authors
- **Iman Hosseini**<sup>1,2,*</sup>
- **Marko Grobelnik**<sup>1</sup>
- **Elke Deckers**<sup>3,4</sup>
- **Dunja Mladenić**<sup>1,2</sup>

### Affiliations
1. Department for Artificial Intelligence, Jožef Stefan Institute, Jamova Cesta 39, Ljubljana 1000, Slovenia
2. Jožef Stefan International Postgraduate School, Jamova Cesta 39, Ljubljana 1000, Slovenia
3. Department of Mechanical Engineering, KU Leuven, Celestijnenlaan 300, Leuven 3001, Belgium
4. Flanders Make@KU Leuven, Belgium

<sup>*</sup> *Corresponding author: iman.hosseini@ijs.si*

**Target Conference:** CIRP Conference on Manufacturing Systems (CMS)