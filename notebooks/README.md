# Notebooks — CIRP 2027 study

Revised scope (2026-11-07): unlabeled LeJEPA pre-training, latent prediction-error scoring, selection of 300 part
records for simulated inspection, defect discovery against PCA reconstruction-error and random selection, and
computational latent-space diagnostics. A downstream Random Forest, SHAP explanations and expert/human evaluation
are **not** part of the current experiment.

| notebook | status | role |
| --- | --- | --- |
| `01_architecture_original.ipynb` | **archival / reference** | First development version (random row split, architecture exploration). Kept unchanged as development history; it is not the current experimental specification and not the source of reported results. |
| `02_architecture_updated.ipynb` | **primary experiment** | Corrected protocol: S14 only, group-aware pool/tuning/test split, pool-only preprocessing, block-masked LeJEPA with SIGReg, exact latent-error scoring, top-300 selection, PCA and random baselines, five seeds, experiments A/B/D/E/F. STEP 11 exports models, preprocessing, splits, scores and selections. |
| `03_lejepa_latent_diagnostics.ipynb` | **diagnostic supplement** | Loads the STEP 11 artifacts of 02 without retraining; latent integrity checks, 2-D PCA of the embeddings, descriptive defect patterns, nearest neighbours, production episodes, and the selections in latent space. |
| `archive_removed_human_evaluation/` | **inactive** | The pre-cleanup version of 02 (with Random Forest, SHAP, Experiment C and the expert-review packet) and an index of the legacy human-evaluation notebooks. Not part of the revised experiment. |

## How to run

From the repository root, with the project environment (Jupyter can also be started in `notebooks/`; both
notebooks resolve paths from the repository root):

```bash
# 1. primary experiment (about 8 minutes on CPU); writes notebooks/outputs/02_architecture_updated/
.venv/Scripts/python.exe -m jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=1800 notebooks/02_architecture_updated.ipynb
# 2. latent diagnostics (under a minute); needs the artifacts from step 1; writes notebooks/outputs/lejepa_latent_diagnostics/
.venv/Scripts/python.exe -m jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=1800 notebooks/03_lejepa_latent_diagnostics.ipynb
```

`notebooks/outputs/` is **not** ignored by Git: the root rule `outputs/**` only matches a top-level `outputs/`
folder. The exported artifacts are small (about 1.5 MB) and can be committed if they should be versioned; otherwise
re-running 02 regenerates them.

## Outputs

- `outputs/02_architecture_updated/results/`: discovery tables (`baseline_results.csv`, `experiment_A_mask_draws.csv`,
  `experiment_results_per_seed.csv`, `experiment_results_summary.csv`, `final_discovery_per_seed.csv`,
  `final_discovery_summary.csv`, `final_config.json`, `run_summary.csv`). The archived pre-cleanup run wrote a
  different set, including classifier AP, to `working_new_version/results_v2/`.
- `outputs/02_architecture_updated/artifacts/`: `lejepa_final_seed{0..4}.pt`, `preprocessing.npz`, `records.csv`,
  `evaluation_labels.csv`, `pool_acquisition.csv`, `export_manifest.json`.
- `outputs/lejepa_latent_diagnostics/`: figures, tables and `diagnostics_manifest.json`.
