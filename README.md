# Intra-Cycle Joint-Embedding Predictive Architecture with Active Learning for Explainable Anomaly Detection in Injection Moulding

[![Status: Pre-Release (Abstract Stage)](https://img.shields.io/badge/Status-Pre--Release-orange.svg)]()
[![Target: CIRP CMS 2027](https://img.shields.io/badge/Target-CIRP_CMS_2027-blue.svg)]()

## Abstract

Modern manufacturing ecosystems increasingly rely on deep learning for anomaly detection and process monitoring. However, traditional generative architectures often struggle with noisy, high-dimensional industrial sensor data, while standard black-box models lack the transparency required for critical human-in-the-loop decision-making. This paper proposes a novel framework integrating a Latent-Euclidean Joint-Embedding Predictive Architecture (LeJEPA) with active learning and explainable artificial intelligence for discrete manufacturing systems. Rather than treating processes like injection molding as continuous time series, we model each manufacturing cycle as an independent snapshot. By employing an intra-cycle feature-masking strategy, the model learns to predict the latent representations of missing sensor modalities from available concurrent readings. Leveraging LeJEPA’s theoretically grounded variance regularization, we construct a robust, unclasped latent space without the need to reconstruct unpredictable raw sensor noise. To minimize labeling bottlenecks, the system employs an active learning loop where frozen pre-trained networks evaluate the unlabeled dataset, querying human experts only when the Euclidean distance between predicted and ground-truth latent states indicates high epistemic uncertainty. Finally, a downstream classifier is trained on this optimized labeling budget, and feature attribution (SHAP) is applied directly to the unmasked physical inputs. This enables the direct tracing of anomalies back to tangible sensor interactions within a single cycle, providing operators with actionable root-cause explanations devoid of temporal confounding. The proposed methodology aligns with the deep-tech manufacturing paradigm by fostering systems that are adaptive, mathematically grounded, and inherently human-centered.


## Overview
This repository contains the codebase for the upcoming research paper introducing a novel framework for anomaly detection in discrete manufacturing systems (e.g., injection molding). 

Traditional generative architectures struggle with noisy industrial sensor data, and black-box models lack the transparency required for human-in-the-loop engineering. To address these gaps, this project integrates a **Latent-Euclidean Joint-Embedding Predictive Architecture (LeJEPA)** with **Active Learning** and **Explainable AI (SHAP)**. By modeling each manufacturing cycle as an independent snapshot, the system learns physical machine dynamics without reconstructing unpredictable sensor noise.

## Proposed Architecture

```text
            UNLABELED SINGLE MANUFACTURING CYCLE (Snapshot)
                                  │
               ┌──────────────────┴──────────────────┐
               │                                     │
             View 1                                View 2
       (Available Sensors)                    (Masked Sensors)
               │                                     │
               ↓                                     ↓
       Shared MLP Encoder                    Shared MLP Encoder
               │                                     │
               ↓                                     ↓
      z1 (Context Embedding)                z2 (Target Embedding)
               │                                     │
               ↓                                     │
        LeJEPA Predictor                             │
               │                                     │
               ↓                                     │
 z2_pred (Predicted Target Embedding)                │ (Stop-Gradient)
               │                                     │
               └──────────────────┬──────────────────┘
                                  ↓
                         Invariance + SIGReg
                 (Loss: MSE + Variance Regularization)
                                  │
══════════════════════════════════│═════════════════════════════════════
 DOWNSTREAM TASKS                 │
                                  ↓
  ┌──────────────────────────────────────────────────────────────┐
  │                   ACTIVE LEARNING LOOP                       │
  │                                                              │
  │ INPUTS RECEIVED:                                             │
  │ 1. Trained Encoder      ─────────┐                           │
  │ 2. Trained Predictor    ─────────┼─> Calculate Latent Error  │
  │ 3. X_tensor (Full Data) ─────────┘   (Distance: z_target vs  │
  │                                       z_pred on 50% mask)    │
  │                                              │               │
  │                                              ↓               │
  │                      Query Indices (Top 300 uncertain cycles)│
  │                                              │               │
  │                                              ↓               │
  │                            Train RandomForest Classifier     │
  └──────────────────────────────────────────────┬───────────────┘
                                                 │
                                                 ↓
  ┌──────────────────────────────────────────────────────────────┐
  │                   EXPLAINABILITY (SHAP)                      │
  │                                                              │
  │ INPUTS RECEIVED:                                             │
  │ 1. Trained Classifier (RandomForest) ───┐                    │
  │ 2. X_active_learning (Queried Raw Data) ┼─> TreeExplainer    │
  │                                         │                    │
  │                                         ↓                    │
  │                        SHAP Values (Feature Attributions)    │
  │                                         │                    │
  │                                         ↓                    │
  │                           Physical Operating Window &        │
  │                           Global Importance Reports          │
  └──────────────────────────────────────────────────────────────┘

```

The pipeline operates in three distinct phases:

1. **Self-Supervised Pre-Training (LeJEPA):** Utilizes an intra-cycle feature-masking strategy. A Shared MLP Encoder and Predictor network learn to predict the latent representations of missing sensor modalities based on available concurrent readings, stabilized by variance regularization.
2. **Active Learning Loop:** The trained, frozen networks evaluate unlabelled datasets. Cycles with the highest epistemic uncertainty (measured via the Euclidean distance between predicted and true latent states) are queried for expert labeling, drastically reducing annotation bottlenecks.
3. **Physical-Space Explainability (XAI):** A downstream classifier (Random Forest) is trained on the actively queried budget. SHAP (TreeExplainer) is applied directly to the *unmasked physical inputs* to trace anomalies back to tangible root causes (e.g., pressure or temperature variations), devoid of temporal confounding.

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

**Author:** Iman Hosseini (iman.hosseini@ijs.si)

**Affiliation:** Institut Jožef Stefan (Department for Artificial Intelligence - E3)

**Target Conference:** CIRP Conference on Manufacturing Systems (CMS)

