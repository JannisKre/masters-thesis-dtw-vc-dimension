# Figure 5.5 — QP Shattering Capacity, $k = 2$ Fixed (Thesis Notation)

**Solver:** Exact QP via Gurobi (`dtw_intersection` submodule)  
**What was fixed:** $k = 2$ (thesis), i.e. center curves have length 2  
**What varies:** $m$ (thesis), i.e. query-curve length  
**Canonical source:** `dtw_intersection/capacity_results_fixed_m2/second_try/`

> ⚠️ **Code ↔ thesis swap:** In file names and CSV columns, `k` and `m` are swapped.
> A file named `kX_m2.csv` uses **code** `k=X` (= thesis $m=X$) and **code** `m=2` (= thesis $k=2$).

## Files

| File | Thesis $k$ | Thesis $m$ | Code filename |
|------|-----------|-----------|---------------|
| `k2_m2.csv`  | 2  | 2  | code k=2,  m=2 |
| `k3_m2.csv`  | 2  | 3  | code k=3,  m=2 |
| `k4_m2.csv`  | 2  | 4  | code k=4,  m=2 |
| `k5_m2.csv`  | 2  | 5  | code k=5,  m=2 |
| `k6_m2.csv`  | 2  | 6  | code k=6,  m=2 |
| `k7_m2.csv`  | 2  | 7  | code k=7,  m=2 |
| `k8_m2.csv`  | 2  | 8  | code k=8,  m=2 |
| `k9_m2.csv`  | 2  | 9  | code k=9,  m=2 |
| `k10_m2.csv` | 2  | 10 | code k=10, m=2 |
| … | … | … | … |
| `k18_m2.csv` | 2  | 18 | code k=18, m=2 |

See also `dtw_intersection/archived_results/capacity_results_fixed_m2_v1/` for a
superseded first-attempt run of this experiment.

## CSV schema

Same as Figure 5.4 — see [fig5.4_qp_fixed_m2_thesis/README.md](../fig5.4_qp_fixed_m2_thesis/README.md).

## How this data was generated

```bash
cd dtw_intersection
python run_capacity.py --k 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 --m 2
```
(code notation: `--m 2` = thesis $k=2$ fixed; `--k` list = thesis $m$ values)
