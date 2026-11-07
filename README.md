# Label-Efficient Inspection Prioritization in Injection Moulding with Correlation-Aware LeJEPA

![Status: Manuscript in Preparation](https://img.shields.io/badge/Status-Manuscript_in_Preparation-orange.svg)
[![Target: CIRP CMS 2027](https://img.shields.io/badge/Target-CIRP_CMS_2027-blue.svg)](https://cms2027.com/)

**Research focus:** Self-supervised representation learning for **label-free inspection prioritization** in injection moulding, with exploratory analysis of learned latent representations.

**Scope of the revised study:** The experimental framework does **not** include downstream defect classification, SHAP explanations, or formal human-evaluator assessments. Manufacturing expertise informs the study's design and interpretation, but is not treated as a separate evaluation experiment.

## Abstract

In injection moulding, quality inspection is costly and defective products are rare, making labeled data scarce and limiting data-driven quality monitoring. This study investigates a correlation-aware, LeJEPA-inspired self-supervised framework for prioritizing manufacturing parts for inspection without using quality labels during acquisition. The dataset contains 5,230 part records from 2,624 injection-moulding cycles, with 24 retained process variables and 1.15% defective parts. Each observation is treated as a multivariate snapshot rather than a temporal sequence. The framework learns latent representations by predicting the embedding of a complete observation from partially visible process measurements. Because many process variables are strongly correlated, related sensors are masked together to reduce information shortcuts during training. A simplified SIGReg-inspired regularizer discourages representation collapse. Latent prediction error, averaged over the admissible sensor-group masks, is used to rank observations for inspection. Across five random seeds, selecting the 300 highest-ranked parts identified **12.6 ± 1.8** defective parts, compared with **11.0** using PCA reconstruction error and **3.4 ± 1.5** using random selection. This corresponds to approximately **39%** of the defects in the acquisition pool and **3.7 times** as many defects as random selection. Exploratory latent-space analysis reveals product-family structure but no consistent separation between defect types. The findings suggest that latent prediction error can support label-efficient inspection prioritization, while the learned representations do not necessarily identify defect categories or their underlying physical causes.

## Overview

This repository accompanies research targeting the **60th CIRP Conference on Manufacturing Systems (CIRP CMS 2027)**. The central question is whether a correlation-aware, LeJEPA-inspired model can use **unlabeled process measurements** to prioritize a small inspection budget so that more rare defects are discovered than with alternative label-free selection strategies.

The framework has three computational components:

1. **Self-supervised representation learning:** Learn to predict the latent embedding of a complete sensor observation from a partially masked observation. Correlated process variables are masked together.
2. **Inspection prioritization:** Rank acquisition-pool part records by latent prediction error and select the highest-scoring 300 records for simulated inspection.
3. **Retrospective evaluation and latent-space diagnostics:** Reveal existing quality labels *after selection* to measure defect discovery, compare against PCA and random selection, and explore whether learned embeddings show associations with product families or defect types.

**Observation unit:** A part record is one multivariate process **snapshot**, not a temporal sequence. Although records originate from manufacturing cycles, the model does not predict future time steps. Multiple part records may belong to the same cycle and can share sensor measurements.

**Inspection protocol:** This is a **single-round, pool-based simulated inspection**. No human labels new parts for the experiment: previously recorded quality labels are hidden during acquisition and revealed only to score the selected records.

## Architecture

```text
                  INJECTION-MOULDING PART RECORD
                     24 standardized variables
                        (6 sensor blocks)
                                |
                  Correlation-aware block masking
                                |
                    +-----------+-----------+
                    |                       |
               CONTEXT VIEW             TARGET VIEW
              [x * mask, mask]          [x, all ones]
                 48 inputs                48 inputs
                    |                       |
                    +----- SHARED MLP ENCODER
                          48 -> 64 -> 32 -> 8
                              |         |
                          z_context   z_target
                            (8-D)       (8-D)
                              |
                         MLP PREDICTOR
                           8 -> 32 -> 8
                              |
                            z_pred ------> latent prediction loss
                                        + simplified SIGReg-inspired
                                          regularization
                              |
                         TRAINED MODEL
                              |
                Evaluate 62 admissible block masks
                              |
             Average ||z_target - z_pred||_2 per record
                              |
                   Rank acquisition-pool records
                              |
                 Select top 300 for inspection
                              |
                    Reveal recorded labels
                              |
             Compare defect discovery with baselines

       Separate exploratory analysis of learned embeddings:
           product-family and defect-type patterns
             (descriptive, not causal explanations)
```

### Representation learning

| Component | Selected configuration |
| --- | --- |
| Process measurements | 24 retained, standardized variables |
| Masking structure | 6 correlation-based sensor blocks |
| Context input | `[x * mask, mask]` (48 values) |
| Target input | `[x, ones]` (48 values) |
| Encoder | Shared MLP, `48 -> 64 -> 32 -> 8` |
| Predictor | MLP, `8 -> 32 -> 8` |
| Latent representation | 8 dimensions |
| Training loss | Latent prediction MSE + simplified SIGReg-inspired penalty |
| Regularization weight | `lambda = 1` |
| Training | AdamW, learning rate `0.003`, weight decay `1e-4`, batch size `256`, up to `150` epochs |
| Model selection | Label-free, group-aware validation and early stopping |

- **Correlated blocks:** Average-linkage hierarchical clustering of absolute correlations computed **from acquisition-pool data only**, using distance `1 - |correlation|` and a cut of `0.3`. The selected configuration produces six blocks.
- **Why blocks matter:** Strongly related sensor readings can reveal masked values if masked independently. Masking correlated sensors together reduces this shortcut.
- **Shared encoder:** Context and full-observation target views pass through the same trainable encoder; this implementation does not employ an EMA teacher or stop-gradient target branch.
- **Regularizer:** The implementation uses a **simplified SIGReg-inspired** isotropy penalty on context and target embeddings (64 random projection directions and 17 evaluation points). It discourages representation collapse; it is not a guarantee against collapse or a claim of exact equivalence to the original LeJEPA implementation.

### Inspection acquisition score

For a part record `x`, the model computes the distance between the **full-view target embedding** and its **predicted embedding** for each admissible sensor-group mask `m`:

```text
score(x) = (1 / 62) * sum_{m in admissible masks}
                         || z_target(x) - z_pred(x, m) ||_2
```

With six sensor blocks, there are **62** admissible masks (`2^6 - 2`), excluding the all-visible and all-hidden cases. In the selected configuration, the score averages over all 62 masks; records are ranked by decreasing score, using record index to break ties.

**Interpretation:** High latent prediction error means the observed measurements are harder to reconcile in the model's latent space. It is **not** a calibrated probability of a defective part, a direct measure of epistemic uncertainty, or an explanation of a physical failure mechanism.

## Dataset and Evaluation Protocol

The study uses the **Korea AI Manufacturing Platform (KAMP)** injection-moulding dataset, restricted in this experiment to machine **S14**.

| Property | Value |
| --- | ---: |
| Part records | 5,230 |
| Manufacturing-cycle groups | 2,624 |
| Defective part records | 60 (1.15%) |
| Gas defects | 35 |
| Short-shot defects | 15 |
| Start-up defects | 10 |
| Original / retained process variables | 26 / 24 |
| Correlation-based sensor blocks | 6 |
| Inspection budget | 300 part records |

The selected preprocessing removes `Barrel_Temperature_7` (constant) and `Switch_Over_Position` (near-constant). Standardization parameters and sensor-correlation grouping are fitted using the acquisition pool, not the held-out partitions.

Records sharing the same `(EQUIP_CD, TimeStamp)` manufacturing cycle remain in the same partition to reduce leakage between paired parts.

| Partition | Part records | Defective records | Role |
| --- | ---: | ---: | --- |
| Acquisition pool | 2,940 | 32 | Self-supervised training and inspection prioritization |
| Tuning / validation | 982 | 13 | Development-time assessment and configuration comparisons |
| Held-out test | 1,308 | 15 | Reserved partition for appropriately separated analyses |

Within the acquisition pool, an additional **label-free, group-aware** split is used for checkpoint validation and early stopping. The selection budget of 300 records is approximately 10% of the acquisition pool; it does **not** imply 300 distinct manufacturing cycles.

Known quality labels are used to **evaluate defect discovery and describe latent-space structure after training/selection**, not to train the representation model or determine acquisition rankings.

## Selection Strategies

All methods select 300 part records from the **same acquisition pool**, without consulting their quality labels during selection.

| Method | Ranking / selection rule |
| --- | --- |
| **LeJEPA-inspired (proposed)** | Highest average latent prediction error across all 62 valid group masks |
| **PCA reconstruction error** | Largest reconstruction error of standardized variables using a pool-fitted PCA model with three components |
| **Random selection** | Uniform random sampling of 300 pool records |

**Primary outcome:** Number of defects among the 300 selected records. This is an **inspection-prioritization / defect-discovery** comparison, **not** a comparison of supervised classifiers.

## Experimental Results

The following results reflect the reported **five-seed** comparison for the selected exact-mask scoring configuration. Defect counts are evaluated on the acquisition pool after the label-free selections are fixed.

| Defects discovered among 300 inspected records | LeJEPA-inspired | PCA error | Random |
| --- | ---: | ---: | ---: |
| **All defects (mean ± SD)** | **12.6 ± 1.8** | 11.0 ± 0.0 | 3.4 ± 1.5 |
| Gas defects (mean) | 5.2 | 5.0 | 2.2 |
| Short-shot defects (mean) | 3.4 | 2.0 | 1.2 |
| Start-up defects (mean) | 4.0 | 4.0 | 0.0 |

On average, the proposed score discovers about **39% of the acquisition pool's 32 defective records** using an inspection budget of 300 records. It finds **3.7× as many defects as random selection**. PCA reconstruction error is also a strong baseline (11.0 defects on average); these results should not be interpreted as proving broad or statistically established superiority over PCA.

### Design choices examined

The experimental work includes comparisons of sampled versus exhaustive mask scoring, feature retention, latent dimension, regularization weight, and encoder width. The reported selected design uses **all 62 masks**, an **8-dimensional** latent representation, `lambda = 1`, and encoder hidden widths `64/32`. These experiments are controlled configuration comparisons, not proof of universal hyperparameter optimality.

## Exploratory Latent-Space Diagnostics

The latent-space analysis is **descriptive** and separate from acquisition scoring. The paper reports **product-family structure** in the learned representations, but **no consistent separation of defect types**.

The companion diagnostics notebook (`notebooks/03_lejepa_latent_diagnostics.ipynb`) is intended to document reproducible checks such as embedding integrity, low-dimensional visualizations, label overlays *after model training*, and the relation between selected records and the learned representation space. Such plots should be interpreted cautiously:

- A visible cluster can reflect **product-family or operating-condition differences**, not necessarily a defect mechanism.
- Defects are rare, and individual defect types have small sample counts.
- Two-dimensional projections can distort distances and separation in the original latent space.
- Latent proximity and prediction error do **not** establish root causes, physical explanations, or causal relationships.

No formal human-expert ratings or inspection of SHAP explanations are used as evaluation results in the revised study. Domain-expert input is limited to research discussion and interpretation.

## Reproducing the Experiments

### Notebook organization

The **intended final notebook layout** after the repository cleanup is:

| Notebook | Role |
| --- | --- |
| `notebooks/01_architecture_original.ipynb` | Earlier exploratory architecture; retained as historical reference, not the source of the final results |
| `notebooks/02_architecture_updated.ipynb` | Main LeJEPA experiment, acquisition, PCA/random baselines, multi-seed defect-discovery results, and relevant ablations |
| `notebooks/03_lejepa_latent_diagnostics.ipynb` | Computational exploration of learned latent representations; no supervised classifier or formal expert assessment |

> **Repository synchronization note:** This README describes the revised research scope and target notebook organization. Verify that the notebook cleanup and creation of `03_lejepa_latent_diagnostics.ipynb` have been committed before treating all paths above as available in the published repository.

### Environment

A starting environment for the computational notebooks can be prepared with:

```bash
python -m pip install jupyterlab numpy pandas torch scipy scikit-learn matplotlib seaborn
```

Use the notebook's actual import cells or the repository's environment/requirements file, when available, as the authority for the exact dependencies and versions. **SHAP is not required** for the revised framework.

### Input data

The existing main notebook expects:

```python
DATA_PATH = '../data/labeled_data_preprocessed.csv'
```

This path is relative to a notebook run from `notebooks/` within a repository that contains a sibling `data/` directory. If your layout differs, update the notebook's path configuration. Data access and redistribution are subject to KAMP source permissions.

Run the **main experimental notebook in cell order**. To reproduce the latent diagnostics, first ensure that its required model checkpoint, preprocessing information, split assignments, and acquisition scores are available from the main experiment. The diagnostics notebook should reuse those artifacts rather than silently training a different model.


## Authors and Affiliations

1. **Seyed Iman Hosseini**<sup>1,2,*</sup>
2. **Lorenzo Seghesio**<sup>3,4</sup>
3. **Marko Grobelnik**<sup>1</sup>
4. **Konstantinos Gryllias**<sup>3,4</sup>
5. **Elke Deckers**<sup>3,4</sup>
6. **Dunja Mladenić**<sup>1,2</sup>

**Affiliations**

1. Department for Artificial Intelligence, Jožef Stefan Institute, Jamova Cesta 39, Ljubljana 1000, Slovenia
2. Jožef Stefan International Postgraduate School, Jamova Cesta 39, Ljubljana 1000, Slovenia
3. Mecha(tro)nic System Dynamics (LMSD), Department of Mechanical Engineering, KU Leuven, Celestijnenlaan 300, Leuven 3001, Belgium
4. Flanders Make@KU Leuven, Belgium

<sup>*</sup>Corresponding author: **iman.hosseini@ijs.si**

**Target venue:** 60th CIRP Conference on Manufacturing Systems (CIRP CMS 2027), *Procedia CIRP*.
