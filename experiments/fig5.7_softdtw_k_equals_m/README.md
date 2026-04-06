# Figure 5.7 — Soft-DTW Shattering Capacity, $k = m$ Diagonal

**Solver:** Soft-DTW gradient descent (`soft_dtw_balls` submodule)  
**What was fixed:** $k = m$ (thesis notation, swap-transparent for the diagonal)  
**What varies:** $k = m \in \{1, 2, \ldots, 9\}$  
**Canonical source:** `soft_dtw_balls/results2/results2/`

> ⚠️ **Code ↔ thesis swap:** For the $k=m$ diagonal the swap is transparent (same value both ways).

## Files

| Files | $k = m$ | Runs |
|-------|---------|------|
| `witnesses_k1_m1_run{1,2,3}.csv` | 1 | 3 independent runs |
| `witnesses_k2_m2_run{1,2,3}.csv` | 2 | 3 independent runs |
| … | … | … |
| `witnesses_k9_m9_run{1,2,3}.csv` | 9 | 3 independent runs |
| `run.log` | all | server stdout log |

## CSV schema

Each file is a long-format witness CSV (one row per subset of the best-shattered
set found in that run), plus a `__POINTS__` header row:

| Column | Description |
|--------|-------------|
| `k` | Code query length = thesis $m$ (= thesis $k$ on the diagonal) |
| `m` | Code center length = thesis $k$ |
| `run_id` | Run index (1–3) |
| `d_max` | Largest shattered set size found |
| `points_json` | JSON: the shattered point set (on `__POINTS__` row) |
| `subset` | Comma-separated 1-based indices of the inside set |
| `witness_json` | JSON: witness center $P$ for this subset |
| `delta` | Separating radius $\Delta$ (hard-DTW validated) |

## How this data was generated

```bash
cd soft_dtw_balls
python run_sequential_k_equals_m.py \
    --m_values 1 2 3 4 5 6 7 8 9 \
    --num_runs 3 --out_dir results2/results2/
```
(code notation: `--m_values` = thesis $k$ values; since $k=m$ both are equal)
