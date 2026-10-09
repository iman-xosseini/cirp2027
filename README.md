# Label-Efficient And Explainable Defect Detection In Injection Moulding Using Self-Supervised Joint-Embedding Predictive Representations

![Status: Manuscript in Preparation](https://img.shields.io/badge/Status-Manuscript_in_Preparation-orange.svg)
[![Target: CIRP CMS 2027](https://img.shields.io/badge/Target-CIRP_CMS_2027-blue.svg)](https://cms2027.com/)

> **Research status:** The revised experimental protocol, baseline comparisons, and ablation experiments have been run. The qualitative domain-expert assessment of the explanations is being completed; its results are **not yet reported**.

## Abstract

In injection moulding, quality inspection is costly and defects are rare, making labeled data scarce and limiting data-driven quality monitoring. We present a self-supervised framework that learns from unlabeled production cycles, selects parts worth inspecting, and explains downstream classifier predictions in sensor terms. The data consist of 5,230 part records from 2,624 injection-moulding cycles, each described by 24 retained process variables and treated as an independent multivariate snapshot rather than part of a time series. From each observation, we hide a subset of sensors and train the model to predict a compact latent representation of the complete observation from the remaining measurements, learning relationships among machine variables without reconstructing raw readings. This adapts the core LeJEPA idea to discrete manufacturing using a modified architecture for multivariate sensor snapshots. Individual sensor masking is problematic because 19 of 24 sensors have a partner with an absolute correlation above 0.9, allowing hidden measurements to be inferred from correlated visible sensors. Masking whole correlated groups reduces this information shortcut. A simplified SIGReg-inspired regularizer discourages representation collapse, enabling latent prediction error to rank unlabeled parts for inspection. Across five random seeds on a dataset containing 1.15% defective parts, selecting the 300 highest-ranked parts identified 12.6 ± 1.8 defects, compared with 11.0 for PCA reconstruction-error selection and 3.4 ± 1.5 for random selection. This corresponds to approximately 39% of the defects in the acquisition pool. A Random Forest trained on the acquired labels uses the original process variables, allowing Tree SHAP explanations in terms of measurable sensor quantities. On the held-out test partition, the proposed LeJEPA-based strategy achieved the highest mean average precision (AP) of 0.152, compared with 0.114 for PCA and 0.048 for random selection.

## Overview

This repository accompanies a study targeting the **60th CIRP Conference on Manufacturing Systems (CIRP CMS 2027)**. It investigates whether **LeJEPA-inspired self-supervised representation learning** can prioritize rare defective injection-moulded parts for inspection without using quality labels during acquisition, and whether the acquired labels support explainable downstream defect classification.

Each observation is a **snapshot of process measurements**, not a temporal sensor sequence. The framework has four stages:

1. **Self-supervised representation learning:** A shared encoder and predictor learn to match the full-observation latent representation from a context in which correlated sensor groups are hidden.
2. **Label-efficient sample acquisition:** A label-free latent prediction-error score ranks candidate part records; the 300 highest-ranked records are selected for simulated inspection.
3. **Defect classification:** A class-weighted Random Forest is trained on the selected records' revealed pass/fail labels and **original process variables**, not LeJEPA embeddings.
4. **Explainability and expert assessment:** Tree SHAP attributes the Random Forest's predictions to process variables. Six explanations have been prepared for qualitative review by an injection-moulding expert; responses are pending.

**Important distinction:** This is a *single-round, pool-based simulated inspection* experiment. Selection does not involve an expert labeling 300 previously unknown parts: existing quality labels are revealed only after the selected indices have been determined.

## Proposed Architecture

```text
INJECTION-MOULDING PART RECORD
24 standardized process sensors (6 correlation-based blocks)
                         |
             Correlated block masking
                         |
             +-----------+-----------+
             |                       |
       CONTEXT VIEW             TARGET VIEW
     [x * mask, mask]           [x, all ones]
        48 inputs                48 inputs
             |                       |
             +---- SHARED MLP ENCODER +
                  48 -> 64 -> 32 -> 8
                    LayerNorm + GELU
             |                       |
        z_context                 z_target
          (8-D)                     (8-D)
             |                       |
      MLP PREDICTOR                  |
        8 -> 32 -> 8                 |
             |                       |
           z_pred ------------------+
                         |
          MSE(z_pred, z_target) + lambda * R_iso
          R_iso = 0.5 * [SIGReg(z_context) + SIGReg(z_target)]
                         |
                  TRAINED MODEL
                         |
       LATENT PREDICTION-ERROR ACQUISITION
        Average L2 distance over all 62 valid block masks
                         |
           Rank acquisition-pool part records
                         |
             TOP 300 PARTS SELECTED
                  Reveal labels
                         |
               RANDOM FOREST
         300 trees; original sensor variables
                         |
                   TREE SHAP
       Sensor attributions and measured values
                         |
          QUALITATIVE EXPERT REVIEW
          6 held-out cases (pending responses)
```

### Representation learning

- **Input:** 24 retained process sensors; masks are supplied explicitly, giving 48 inputs to the shared encoder.
- **Correlated blocks:** Average-linkage hierarchical clustering of pool-only absolute sensor correlations, using distance `1 - |correlation|` and a cut of `0.3`, yields **6 blocks**.
- **Views:** The context input contains hidden blocks plus a visibility mask. The target input contains all sensors and an all-visible mask. Both use the **same trainable encoder**, with no stop-gradient or EMA teacher.
- **Networks:** Encoder `48 -> 64 -> 32 -> 8`; predictor `8 -> 32 -> 8`; **6,224 trainable parameters** in total.
- **Loss:** Latent prediction mean squared error plus an isotropy penalty (`lambda = 1`). The implementation is a **simplified SIGReg-inspired regularizer** applied to both context and target embeddings, using 64 random projection directions and 17 evaluation points. It *discourages*, rather than guarantees the elimination of, representation collapse.
- **Training:** AdamW (`learning_rate = 0.003`, `weight_decay = 1e-4`), batch size 256, up to 150 epochs, with group-aware label-free checkpoint validation and early stopping.

### Acquisition score

For part record `x`, let `z_target(x)` be its full-view embedding and `z_pred(x, m)` the predicted embedding under block mask `m`. Its score is

```text
score(x) = (1 / 62) * sum_over_valid_masks ||z_target(x) - z_pred(x, m)||_2
```

All `2^6 - 2 = 62` masks excluding the all-hidden and all-visible patterns are evaluated for the **final selected configuration**. Parts are ranked in descending score order, with record index used to break ties. **A high score indicates latent prediction inconsistency, not a calibrated defect probability or a direct measure of epistemic uncertainty.**

## Dataset and Evaluation Protocol

The data originate from the **Korea AI Manufacturing Platform (KAMP)** injection-moulding dataset. The updated experiment restricts analysis to machine **S14**:

| Property                             |            Value |
| ------------------------------------ | ---------------: |
| Part records analyzed                |            5,230 |
| Distinct moulding-cycle groups       |            2,624 |
| Defective part records               |       60 (1.15%) |
| Gas defects                          |               35 |
| Short-shot defects                   |               15 |
| Start-up defects                     |               10 |
| Original / retained sensor variables |          26 / 24 |
| Correlation-based sensor blocks      |                6 |
| Inspection budget                    | 300 part records |

`Barrel_Temperature_7` (constant) and `Switch_Over_Position` (near-constant) are removed in the selected configuration. The retained variables are standardized using a scaler **fitted to the acquisition pool only**.

Records from the same `(EQUIP_CD, TimeStamp)` manufacturing cycle are kept in the same partition to reduce leakage between paired parts. The split in the updated notebook is:

| Partition           | Part records | Defective records | Role                                                         |
| ------------------- | -----------: | ----------------: | ------------------------------------------------------------ |
| Acquisition pool    |        2,940 |                32 | Self-supervised training and sample selection                |
| Tuning / validation |          982 |                13 | Compare configurations and evaluate downstream AP during development |
| Held-out test       |        1,308 |                15 | Final downstream evaluation                                  |

A separate label-free, group-aware validation split inside the pool is used for LeJEPA checkpoint selection. The acquisition budget is **300 part records (about 10% of the pool)**, not necessarily 300 distinct moulding cycles; paired parts can share sensor measurements.

## Selection Strategies

All methods choose 300 records from the **same acquisition pool**, without using their quality labels to determine the selection:

| Strategy                     | Selection criterion                                          |
| ---------------------------- | ------------------------------------------------------------ |
| **LeJEPA (proposed)**        | Largest average latent prediction error across the 62 valid block masks |
| **PCA reconstruction error** | Largest error reconstructing standardized sensor vectors from a pool-fitted PCA model with 3 components |
| **Random sampling**          | Uniform random selection of 300 pool records                 |

The same downstream Random Forest evaluation procedure is used for each selected training set. **Defect discovery** (number of defects among selected parts) is the primary evaluation outcome; **downstream average precision (AP)** on unseen parts is secondary. AP summarizes precision and recall across score thresholds and is useful when defects are rare.

## Main Experimental Results

Results below are **means and standard deviations across five random seeds**, using the final exact-scoring LeJEPA configuration.

| Metric                               |            LeJEPA |           PCA |        Random |
| ------------------------------------ | ----------------: | ------------: | ------------: |
| Defects discovered / 300 inspected   |    **12.6 ± 1.8** |    11.0 ± 0.0 |     3.4 ± 1.5 |
| Gas defects discovered (mean)        |               5.2 |           5.0 |           2.2 |
| Short-shot defects discovered (mean) |               3.4 |           2.0 |           1.2 |
| Start-up defects discovered (mean)   |               4.0 |           4.0 |           0.0 |
| Tuning / validation AP               | **0.400 ± 0.012** | 0.209 ± 0.006 | 0.093 ± 0.072 |
| Held-out test AP                     | **0.152 ± 0.047** | 0.114 ± 0.022 | 0.048 ± 0.022 |

The proposed acquisition score finds approximately **3.7 times as many defects as random selection** and approximately **39% of the 32 defects in the acquisition pool** within 300 inspected records. PCA also provides a strong defect-discovery baseline, so its smaller gap from LeJEPA should not be interpreted as conclusive superiority. On the held-out test partition, AP estimates have substantial uncertainty because only **15 defective records** are available. Approximate prevalence-based chance levels of AP are 0.013 on tuning data and 0.011 on test data.

### Influence of design choices

The updated notebook compares sampled versus exact acquisition scoring, retained near-constant features, part identity in the Random Forest, latent dimensionality, SIGReg weight, and encoder width. The final configuration adopts **exact mask enumeration** and retains the 8-dimensional latent space, `lambda = 1`, and `64/32` encoder hidden widths. The study reports these comparisons as controlled single-factor variations, not as proof of universally optimal hyperparameters.

## Explainability and Expert Review

A Random Forest is fitted to the **300 selected, labeled part records** using standardized original sensor variables. Tree SHAP provides attribution values for the defect-class prediction. Displayed sensor readings can be converted back to their measured values, and the expert packet includes typical ranges for the corresponding part type. **Engineering units are shown only if independently verified**; the notebook currently leaves the units mapping empty.

The prepared expert-review packet contains **six held-out cases** (three defective, three non-defective). For each case, the expert sees the recorded quality outcome, predicted defect probability, and the three most influential sensors with values, comparison ranges, and attribution directions. The response options are **Yes / Partly / No / Insufficient information**, plus a brief comment.

**Status: expert responses are pending.** This is a limited *qualitative plausibility review* of the downstream classifier explanations. It is **not blinded**, has no scrambled-sensor control, and does not establish causal correctness, root causes, or operational actionability.

## Reproducing the Experiments

The main experimental implementation is **`02_architecture_updated.ipynb`**. The earlier **`01_architecture_original.ipynb`** is retained as the exploratory version and should **not** be used as the source of the final reported results.

The notebook imports Python packages including `numpy`, `pandas`, `torch`, `scipy`, `scikit-learn`, `shap`, `matplotlib`, and `seaborn`. You can prepare a suitable Python environment with:

```bash
python -m pip install jupyterlab numpy pandas torch scipy scikit-learn shap matplotlib seaborn
```

The updated notebook currently expects the input CSV at:

```python
DATA_PATH = '../data/labeled_data_preprocessed.csv'
```

This relative path assumes the notebook is executed from a directory immediately below `data/` (for example, `notebooks/` next to `data/`). **Place the dataset accordingly or change `DATA_PATH` in STEP 0**. Dataset distribution and access depend on the KAMP source and applicable permissions.

Run the updated notebook **in cell order**, from STEP 0 through the final summary. Key stages are STEP 1 (group-aware splitting and preprocessing), STEP 2–3 (LeJEPA training), STEP 4 (selection and baselines), STEP 5 (SHAP), STEP 6/9/10 (multi-seed experiments and final evaluation), and STEP 7 (expert-review materials). Notebook section numbers are retained from its development history and do not appear strictly in numerical order.

The notebook writes experimental outputs to `results_v2/`, including, among others:

- `baseline_config.json` and `baseline_results.csv`
- `experiment_A_mask_draws.csv`
- `experiment_C_part_identity.csv`
- `experiment_results_per_seed.csv` and `experiment_results_summary.csv`
- `final_config.json`, `final_test_results_per_seed.csv`, and `final_test_results_summary.csv`
- `run_summary.csv`

Expert-review files are generated in `expert_review/`:

- `expert_review_packet.md` — case descriptions and explanations for review
- `expert_review_form.csv` — blank (until completed) expert response form
- `case_key.csv` — case traceability and recorded outcomes

**Note:** The lists above describe notebook inputs and generated artifacts; they do not imply that all data or generated outputs are committed to the repository.

## Study Status

- [x] LeJEPA-inspired block-masked representation learning
- [x] Group-aware partitioning and pool-only preprocessing
- [x] Exact latent-error acquisition with 62 masks
- [x] Budget-matched PCA and random selection baselines
- [x] Five-seed comparisons, downstream AP, and design-choice experiments
- [x] Random Forest and Tree SHAP explanations
- [x] Six-case expert-review materials prepared
- [ ] Domain-expert responses collected and assessed
- [ ] Expert-assessment findings incorporated into the conference manuscript

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