# Multiple-Depot Vehicle Routing and Hospital Pharmacy Supply Chain Optimization

> **Note:** GitHub’s default Markdown renderer does not support LaTeX formulas in a repository README. To preserve and display mathematical expressions, choose one of the following approaches:
>
> 1. **Serve via GitHub Pages**:
>
>    * Enable Jekyll in your repo by creating an `index.md` (or using your README) with the following front matter at the top:
>
>      ```yaml
>      ---
>      title: "Project Documentation"
>      markdown: kramdown
>      kramdown:
>        math_engine: mathjax
>      ---
>      ```
>    * Write formulas with `$$ ... $$` or inline `$ ... $`. GitHub Pages will render them with MathJax.
>
> 2. **Use a browser extension** that injects MathJax/KaTeX into GitHub (e.g., “MathJax Plugin for GitHub”).
>
> 3. **Embed equations as images**:
>
>    * Export each formula as an image (PNG/SVG) from a LaTeX editor.
>    * Embed in Markdown: `![equation](path/to/equation.png)`.

## Introduction

Efficient distribution of pharmaceuticals to hospitals demands accurate, timely routing with capacity and time constraints. MDVRP extends the classic Vehicle Routing Problem (VRP) by allowing multiple depots. Key objectives:

* Minimize total transportation cost
* Ensure on-time delivery of required quantities
* Respect vehicle capacity, working hours, and storage conditions

---

## System Requirements

* Python 3.8+
* IBM DOcplex (`pip install docplex`)
* NetworkX (`pip install networkx`)
* NumPy, pandas, matplotlib

---

## Installation & Execution

1. Clone the repository:

   ```bash
   git clone https://github.com/username/MDVRP-Pharmacy-Logistics.git
   cd MDVRP-Pharmacy-Logistics
   ```
2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```
3. Run the optimization script:

   ```bash
   python run_optimization.py --data data/hospital_pharmacy.xlsx
   ```
4. Results are saved in the `results/` directory as tables and figures.

---

## Repository Structure

```
MDVRP-Pharmacy-Logistics/
├── data/                  # Input files (Excel, CSV)
├── notebooks/             # Experimental Jupyter notebooks
├── scripts/
│   ├── run_optimization.py  # Main execution script
│   └── utils.py             # Helper functions (distance matrix, etc.)
├── results/               # Generated outputs (tables, plots)
├── requirements.txt       # Python dependencies
└── README.md              # Project documentation
```

---

## Methodology & Algorithms

This section details the end-to-end approach, from data preprocessing to advanced post-optimization.

### 1. Data Preprocessing & Initialization

* **Data loading**: Read coordinates, demand $q_i$, and service time $s_i$ from Excel.
* **Distance matrix**: Compute Euclidean distances between all depots and hospitals.
* **Model parameters**: Define number of vehicles $K$, capacity $Q$, speed $v$, and maximum working time $T_{max}$.
* **Heuristic warm start**: Generate initial routes using the Nearest Neighbor algorithm from each depot.

### 2. MILP Model Formulation

* **Decision variables**:

  * Binary $x_{ijk}$: 1 if vehicle $k$ travels from node $i$ to $j$.
  * Continuous $u_{ik}$: Remaining load of vehicle $k$ after serving node $i$.
* **Objective function**:

  $$
    \min \sum_{k=1}^{K} \sum_{i=0}^{N} \sum_{j=0}^{N} d_{ij} \, x_{ijk}
  $$

  where $d_{ij}$ is the distance between nodes $i$ and $j$.

### 3. Core Constraints

1. **Start and return**:
   $  \sum_{j=1}^{N} x_{0jk} = 1, \quad \sum_{i=1}^{N} x_{i0k} = 1$
2. **Service requirement**:
   $  \sum_{k=1}^{K} \sum_{i=0}^{N} x_{i j k} = 1, \quad \forall j=1\dots N$
3. **Capacity**:
   $  u_{jk} = u_{ik} + q_j x_{ijk}, \quad u_{0k}=0; \quad u_{jk} \le Q$
4. **Working time limit**:
   $  \sum_{i=0}^{N} \sum_{j=0}^{N} \left(\frac{d_{ij}}{v} + s_j\right) x_{ijk} \le T_{max}$
5. **Sub-tour elimination** (Miller–Tucker–Zemlin):
   $  u_{jk} \ge u_{ik} + q_j - Q (1 - x_{ijk})$

### 4. Solution Strategy & Enhancements

* **Solver**: IBM CPLEX via DOcplex API.
* **Branch-and-bound** with **cutting planes** to reduce search space.
* **Warm start** from heuristic to speed up convergence.
* **Problem decomposition**: For larger instances, partition by geographic region and combine sub-solutions.
* **Post-optimization**: Apply 2-opt and 3-opt local search to refine routes.

### 5. Validation & Performance

* **Cost comparison**: MILP vs. heuristic, average gap < 2%.
* **Run time**: < 15 minutes for $N=15, K=4$.
* **Robustness**: 5 runs with ±10% demand variation, gap < 1.5%.

---

## Input Data

* `hospital_pharmacy.xlsx`: Coordinates, demand, and service time for 15 hospitals.
* Depot information (2 depots), vehicle count, capacity, and time limits.

---

## Results & Analysis

* **Total distance**: 4015 km
* **Vehicles used**: 4 (2 depots × 2 vehicles)
* **Detailed routing**: 19 distinct routes

**Route map visualization**

\[Insert route map image here]

**Cost and time comparison chart**

\[Insert comparison chart here]

---

## Limitations & Future Work

* **Scalability**: Computationally challenging for large-scale instances.
* **Static assumptions**: Does not account for dynamic traffic conditions.
* **Reverse logistics**: No handling of expired product returns.

**Future directions**:

* Real-time routing with live traffic data.
* Integration of reverse logistics and temperature-controlled constraints.

---

## References

1. Serrou & Abouabdellah (2016). *Centralized Pharmacy Structure MDVRP in Morocco*.
2. Santoso et al. (2014). *Logistics Cost Analysis*.
3. IBM CPLEX Documentation.
4. Ho et al. (2008); Aras et al. (2011).

---

**Version:** 1.1.0
**Contact:** Nguyễn Thị Lan Anh ([email@domain.com](mailto:email@domain.com))
