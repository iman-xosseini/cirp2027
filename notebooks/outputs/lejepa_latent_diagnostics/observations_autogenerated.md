# Computed observations (auto-generated, facts only)

- Data: 5230 part records (2624 shots), 60 defective; pool / tuning / test = 2940, 982, 1308 records.
- Representation: z_target, 8 dimensions; pool effective rank 2.04; the 2-D visualisation PCA keeps 99.6% of pool latent variance.
- Integrity: 20 / 20 checks pass (scores and selections of all 5 seeds reproduced).
- Nearest pool neighbours (mean share that are defective; pool base rate 0.011): pool defects 0.111 for gas (n=18); pool defects 0.180 for short_shot (n=10); pool defects 0.200 for startup (n=4); tuning defects 0.111 for gas (n=9); tuning defects 0.200 for short_shot (n=2); tuning defects 0.800 for startup (n=2); test defects 0.000 for gas (n=8); test defects 0.000 for short_shot (n=3); test defects 0.400 for startup (n=4); pool pass 0.009; tuning pass 0.008; test pass 0.005.
- Episodes: 60 defective records form 15 episodes (gap > 30 min); start-up: 10 records in 1 episode(s) spanning partitions ['pool', 'test', 'tuning'].
- Selection (seed 0): LeJEPA top-300 holds 15 defective records in 11 shots (155 distinct shots selected); PCA baseline 11 in 9 shots; Jaccard(LeJEPA, PCA) = 0.154.
- All 15 per-seed defect counts equal notebook 02's reported table.
