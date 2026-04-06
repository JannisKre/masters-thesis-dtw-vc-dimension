# Experiments — Data Index

This folder collects all result data used in Chapter 5 of the thesis, organised by figure.
Each subdirectory is named `fig<N>_<solver>_<what_was_fixed>/` and contains a local
`README.md` with precise parameter documentation.

> ⚠️ **Notation reminder:** In the code (file names, CSV columns), the roles of `k` and
> `m` are **swapped** relative to the thesis.
> - Thesis: `k` = center-curve length, `m` = query-curve length
> - Code:   `k` = query-curve length,  `m` = center-curve length
>
> Each subdirectory README translates the file names into thesis notation explicitly.

---

## Figure → Data Map

| Figure | Folder | Solver | What was fixed | Thesis parameters |
|--------|--------|--------|----------------|-------------------|
| 5.4 | [fig5.4_qp_fixed_m2_thesis/](fig5.4_qp_fixed_m2_thesis/) | Exact QP (Gurobi) | $m = 2$ fixed | $m=2$, $k$ varies |
| 5.5 | [fig5.5_qp_fixed_k2_thesis/](fig5.5_qp_fixed_k2_thesis/) | Exact QP (Gurobi) | $k = 2$ fixed | $k=2$, $m$ varies |
| 5.6 | [fig5.6_qp_server/](fig5.6_qp_server/) | Exact QP (Gurobi, HPC) | $m=2$ and $m=3$ sweep | $m \in \{2,3\}$, $k$ varies |
| 5.7 | [fig5.7_softdtw_k_equals_m/](fig5.7_softdtw_k_equals_m/) | Soft-DTW gradient descent | $k = m$ diagonal | $k = m \in \{1,\ldots,9\}$ |
| 5.8 | [fig5.8_softdtw_growth_function/](fig5.8_softdtw_growth_function/) | Soft-DTW gradient descent | $k = m = 3$ fixed | growth function $\Pi(s)$ |

All data are copies of the canonical files that live in the submodules
(`dtw_intersection/` and `soft_dtw_balls/`).  The submodule copies are the
authoritative source; this folder exists purely for convenient, figure-centric
browsing.
