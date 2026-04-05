# VC Dimension of DTW Ball Hypothesis Classes

This repository accompanies the master's thesis

> **"VC Dimension of DTW Ball Hypothesis Classes"**
> Jannis Kremer, 2026

It contains all computational experiments, solvers, and visualisations used in the thesis.

---

## Overview

The **Dynamic Time Warping (DTW)** distance is a widely used similarity measure for time series. The thesis studies the **VC dimension** of the hypothesis class of DTW balls: given sequences of length $k$ as centres and sequences of length $m$ as query points, how large a set can be shattered?

The central object is the **range space** $\mathcal{R}^1_{k,m}$, whose hypotheses are DTW balls

$$b_{\mathrm{dtw}}(P, \Delta) = \{ Q \in \mathbb{R}^m : \mathrm{DTW}(P, Q)^2 \leq \Delta \}$$

parameterised by a centre curve $P \in \mathbb{R}^k$ and radius $\Delta \geq 0$. The main question is: what is $\mathrm{VCdim}(\mathcal{R}^1_{k,m})$?

### Key theoretical results (thesis)

- **Upper bound:** $\mathrm{VCdim}(\mathcal{R}^1_{k,m}) = O(km)$ (via a Warren-type sign-count argument over the $|\mathcal{W}^*_{k,m}|$ optimal warping paths).
- **Lower bound $m=2$ fixed:** $\mathrm{VCdim}(\mathcal{R}^1_{k,2}) \geq k-1$.
- **Lower bound $m=3$ fixed:** $\mathrm{VCdim}(\mathcal{R}^1_{k,3}) \geq k-2$.
- **Computational evidence** suggests these lower bounds are not tight (see results below).

---

## The Separation Problem

The computational experiments reduce to the following decision problem:

**DTW Ball Separation Problem:** Given curves $Q_1, \ldots, Q_s \in \mathbb{R}^m$ and a subset $I \subseteq \{1, \ldots, s\}$, does there exist $P \in \mathbb{R}^k$ and $\Delta \geq 0$ such that
$$\mathrm{DTW}(P, Q_i) \leq \Delta \quad \forall\, i \in I \qquad \text{and} \qquad \mathrm{DTW}(P, Q_j) > \Delta \quad \forall\, j \notin I\;?$$

A point set is **shattered** if every subset is separable. This gives a lower bound $\mathrm{VCdim} \geq s$.

For fixed $k$ and $m$, the problem is solvable in polynomial time in $s$ (degree $2^{k+1}$), via Cylindrical Algebraic Decomposition over the semialgebraic set of DTW ellipsoids. The two solvers in this repo implement more practical algorithms.

---

## Repository Structure

```
masters-thesis-dtw-vc-dimension/
├── dtw_intersection/       ← Submodule: exact QP solver (Gurobi)
├── soft_dtw_balls/         ← Submodule: soft-DTW gradient descent solver (PyTorch)
└── visualizations/         ← Plotting notebooks (matplotlib)
    ├── cost_function/      # DTW cost function geometry
    ├── dtw_balls/          # DTW ball shapes and shattering witnesses
    ├── graphs/             # Graph-based DTW structure
    ├── vc_dimension/       # VC dimension experimental plots
    ├── warping_paths/      # Warping path visualisations
    └── number_of_paths.ipynb
```

### Cloning (with submodules)

```bash
git clone --recurse-submodules https://github.com/JannisKre/masters-thesis-dtw-vc-dimension.git
```

Or, if you already cloned without `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

---

## Component 1: Exact QP Solver (`dtw_intersection/`)

**Repo:** [JannisKre/dtw_intersection](https://github.com/JannisKre/dtw_intersection)

This solver encodes the DTW dynamic programming recursion as a **mixed-integer quadratically constrained program (MIQCP)** and solves it with Gurobi. For each of the $2^s$ subsets, one MIQCP is solved; feasibility of all of them certifies shattering.

**Dependencies:** Gurobi (≥ 9.0) + `numpy`, `matplotlib`, `gurobipy`.  
Academic Gurobi licenses: https://www.gurobi.com/academia/academic-program-and-licenses/

```bash
cd dtw_intersection
pip install -r requirements.txt
python run_capacity.py --k 2 3 4 --m 2 3
```

| Characteristic | Value |
|---|---|
| Correctness | Exact (certifies feasibility) |
| Model size | $O(skm)$ variables per subset |
| Practical limit | $k, m \lesssim 5$; $s \lesssim 9$ |
| Use case | Small parameters; rigorous witness production |

### Key results (QP)

- Confirmed 9 points shattered for $k=7$, $m=2$ — all $2^9 = 512$ subsets feasible.
- Confirmed 8 points shattered for $k = m = 4$.
- Fixed $m=2$: shattering capacity grows linearly in $k$, consistent with the $k-1$ lower bound.

---

## Component 2: Soft-DTW Gradient Descent (`soft_dtw_balls/`)

**Repo:** [JannisKre/SoftDtw](https://github.com/JannisKre/SoftDtw)

This solver replaces the hard DTW minimum with the **soft-DTW distance** (Cuturi & Blondel 2017):

$$\mathrm{dtw}^\gamma(P, Q) = \min^\gamma_{A \in \mathcal{A}_{k,m}} \langle A, \Delta(P,Q) \rangle, \qquad \min^\gamma(a_i) = -\gamma \log \textstyle\sum_i e^{-a_i/\gamma}$$

For $\gamma > 0$ the distance is smooth and differentiable; Adam gradient descent optimises the centre $P$ and threshold $\Delta$ jointly over a hinge separation loss. Multiple random restarts mitigate non-convexity.

**Dependencies:** `torch >= 2.0`, `numpy`, `pandas`, `matplotlib`.

```bash
cd soft_dtw_balls
pip install -r requirements.txt               # GPU
# or: pip install -r requirements-cpu.txt     # CPU only
python run_sequential_k_equals_m.py --k 3 4 --runs 3
```

| Characteristic | Value |
|---|---|
| Correctness | Approximate ($\gamma$-smoothed DTW) |
| Cost per subset | $O(RTskm)$ ($R$ restarts, $T$ epochs) |
| Practical limit | $k, m \lesssim 20$ routinely |
| Use case | Large parameters; growth function estimation |

### Key results (soft-DTW)

- For $k = m = 3$: the growth function $\Pi(s)$ grows as $2^s$ for $s \leq 6$ and falls below for $s \geq 7$, confirming $\mathrm{VCdim} \geq 6$.
- For $k = m \in \{1, \ldots, 9\}$: capacity ≈ $2k$ (gradient descent estimate).
- A $k=m=15$ separation task solved successfully where the exact QP times out.

---

## Component 3: Visualisations (`visualizations/`)

Jupyter notebooks that produce the thesis figures:

| Notebook | Description |
|---|---|
| `cost_function/cost_function.ipynb` | DTW cost function surface and ellipsoid slices |
| `dtw_balls/dtw_balls.ipynb` | 2D DTW ball geometry; shattering witness illustration |
| `dtw_balls/dtw_ball_visualization.ipynb` | Interactive DTW ball visualisation |
| `graphs/graphs.ipynb` | Graph structure of warping paths |
| `vc_dimension/balls.ipynb` | VC dimension experimental lower bound plots |
| `vc_dimension/vc_dim.ipynb` | Capacity vs $(k, m)$ summary plots |
| `warping_paths/warping_paths.ipynb` | Optimal warping path enumeration and illustrations |
| `number_of_paths.ipynb` | $\|\mathcal{W}^*_{k,m}\|$ as a function of $k$, $m$ |

```bash
cd visualizations
pip install -r requirements.txt
jupyter notebook
```

---

## Computational Results Summary

### Fixed query length $m = 2$

Shattering capacity grows linearly in $k$ (verified by QP):

| $k$ | Capacity | Theory lower bound |
|-----|----------|--------------------|
| 2 | 3 | 1 |
| 3 | 4 | 2 |
| 4 | 5 | 3 |
| 5 | 6 | 4 |
| 7 | 9 | 6 |

### Fixed centre length $k = 2$

Capacity stagnates near 5 for tested values of $m$ (theory: $\Theta(\log m)$):

| $m$ | 2 | 3 | 4 | 5 | 6 | 7 |
|-----|---|---|---|---|---|---|
| Capacity | 4 | 5 | 5 | 5 | 5 | 5 |

### Equal lengths $k = m$

| $k = m$ | QP | Soft-DTW |
|---------|-----|---------|
| 1 | 1 | 1 |
| 2 | 4 | 4 |
| 3 | 6 | 6 |
| 4 | 8 | 8 |
| 5–9 | — | ~$2k$ |

---

## Solver Comparison

| | Exact QP (`dtw_intersection`) | Soft-DTW GD (`soft_dtw_balls`) |
|---|---|---|
| **DTW model** | Hard DTW via DP indicator constraints | Soft-DTW ($\gamma > 0$) via autograd |
| **Optimizer** | Gurobi MIQCP branch-and-bound | Adam gradient descent |
| **Correctness** | Exact | Approximate |
| **Detects small separations** | Yes (down to $\varepsilon = 10^{-4}$) | No (misses $\varepsilon \ll \eta$) |
| **Scalable to large $k, m$** | No | Yes |
| **Licence required** | Gurobi (free for academia) | None |

---

## References

- Cuturi, M. & Blondel, M. (2017). *Soft-DTW: a Differentiable Loss Function for Time-Series*. ICML. [arXiv:1703.01541](https://arxiv.org/abs/1703.01541)
- Kingma, D. P. & Ba, J. (2015). *Adam: A Method for Stochastic Optimization*. ICLR.
- Collins, G. E. (1975). *Quantifier Elimination for Real Closed Fields by Cylindrical Algebraic Decomposition*. Springer LNCS.
- Warren, H. E. (1968). *Lower bounds for approximation by nonlinear manifolds*. Trans. AMS 133, 167–178.
- Basu, S., Pollack, R. & Roy, M.-F. (2006). *Algorithms in Real Algebraic Geometry*. Springer.
- Gurobi Optimization (2024). *Gurobi Optimizer Reference Manual*. https://www.gurobi.com