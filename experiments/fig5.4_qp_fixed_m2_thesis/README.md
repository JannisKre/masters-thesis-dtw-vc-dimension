# Figure 5.4 — QP Shattering Capacity, $m = 2$ Fixed (Thesis Notation)

**Solver:** Exact QP via Gurobi (`dtw_intersection` submodule)  
**What was fixed:** $m = 2$ (thesis), i.e. query curves have length 2  
**What varies:** $k$ (thesis), i.e. center-curve length  
**Canonical source:** `dtw_intersection/capacity_results/`

> ⚠️ **Code ↔ thesis swap:** In file names and CSV columns, `k` and `m` are swapped.
> A file named `k2_mX.csv` uses **code** `k=2` (= thesis $m=2$) and **code** `m=X` (= thesis $k=X$).

## Files

| File | Thesis $k$ | Thesis $m$ | Code filename |
|------|-----------|-----------|---------------|
| `k2_m2.csv` | 2 | 2 | code k=2, m=2 |
| `k2_m3.csv` | 3 | 2 | code k=2, m=3 |
| `k2_m4.csv` | 4 | 2 | code k=2, m=4 |
| `k2_m5.csv` | 5 | 2 | code k=2, m=5 |
| `k2_m6.csv` | 6 | 2 | code k=2, m=6 |
| `k2_m7.csv` | 7 | 2 | code k=2, m=7 |
| `k2_m8.csv` | 8 | 2 | code k=2, m=8 |

## CSV schema

Each file is a long-format CSV. One special header row (empty `subset`) per run contains the full point set; subsequent rows are per-subset witnesses.

| Column | Description |
|--------|-------------|
| `k` | Code query length = thesis $m$ |
| `m` | Code center length = thesis $k$ |
| `run_id` | Independent repetition index |
| `d_max` | Largest shattered set size found in this run |
| `points_json` | JSON: the shattered point set (on the `__POINTS__` header row) |
| `subset` | Comma-separated 1-based inside-set indices (empty = header row) |
| `witness_json` | JSON: witness center $P$ for this subset (length = code `m` = thesis $k$) |
| `delta` | Separating radius $\Delta$ |

## How this data was generated

```bash
cd dtw_intersection
python run_capacity.py --k 2 --m 2 3 4 5 6 7 8
```
(code notation: `--k 2` = thesis $m=2$ fixed; `--m` list = thesis $k$ values)
