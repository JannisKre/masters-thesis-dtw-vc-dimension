# Figure 5.6 — QP Shattering Capacity, Server Runs

**Solver:** Exact QP via Gurobi, run on HPC cluster  
**What was varied:** $k \in \{2,\ldots,6\}$, $m \in \{2, 3\}$ (thesis notation); plus $k=m$ diagonal  
**Canonical source:** `dtw_intersection/results_from_server/`

> ⚠️ **Code ↔ thesis swap:** File names follow code convention (`k` = query length = thesis $m`).

## Subdirectories

### `capacity_results_big_sphere_gaussian_20260319_103528/`

Long HPC run with `sphere-gaussian` sampler (points sampled near the unit sphere
rather than uniformly). Contains the most statistically robust estimates.

| File | Thesis $k$ | Thesis $m$ | Sampler |
|------|-----------|-----------|---------|
| `k2_m2.csv` | 2 | 2 | sphere-gaussian |
| `k2_m3.csv` | 3 | 2 | sphere-gaussian |
| `k3_m2.csv` | 2 | 3 | sphere-gaussian |
| `k3_m3.csv` | 3 | 3 | sphere-gaussian |
| `k4_m2.csv` | 2 | 4 | sphere-gaussian |
| `k4_m3.csv` | 3 | 4 | sphere-gaussian |
| `k5_m2.csv` | 2 | 5 | sphere-gaussian |
| `k6_m2.csv` | 2 | 6 | sphere-gaussian |

Each CSV is accompanied by a JSON file (same stem) containing the full run history.

### `capacity_results_k_equals_m_48h/`

48-hour SLURM job for the $k = m$ diagonal (thesis $k = m \in \{3, 4\}$).

| File | Thesis $k = m$ |
|------|----------------|
| `k3_m3.csv` | 3 |
| `k4_m4.csv` | 4 |

## Extended CSV schema

In addition to the standard columns (see [fig5.4 README](../fig5.4_qp_fixed_m2_thesis/README.md)),
server result files include:

| Column | Description |
|--------|-------------|
| `run_index` | Sub-index within a multi-seed run group |
| `is_best_run` | Whether this run achieved the highest `d_max` in its group |
| `base_seed` / `run_seed` | RNG seeds for reproducibility |
| `T` | QP iterations per subset |
| `sampler` | Point-sampling strategy (`sphere-gaussian`) |
| `sampler_params_json` | Sampler hyperparameters |
| `history_json` | Per-attempt history: `d_max` at each sequential step |

## How this data was generated

```bash
# Submitted via SLURM on the university HPC cluster
sbatch slurm_capacity.sh   # see dtw_intersection/slurm_capacity.sh
```
