# Multiple-Depot Vehicle Routing and Hospital Pharmacy Supply Chain Optimization


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

 ![Objective](https://github.com/LanAnh55/Optimize/blob/main/contraints/objective.png).

  where $d_{ij}$ is the distance between nodes $i$ and $j$.

### 3. Core Constraints

1. **Flow Conservation**: Ensure that the flow in and out of each node is balanced for each vehicle.
    ![Objective](https://github.com/LanAnh55/Optimize/blob/main/contraints/constraint%201.png).
   
3. **Customer Assignment**: Ensure that each C is visited exactly once by one vehicle. It prevents multiple vehicles from serving the same customer
   ![Objective](https://github.com/LanAnh55/Optimize/blob/main/contraints/constraints%202.png).
   
5. **CapacityConstraints**: Ensure that the total demand served by each vehicle does not exceed its capacity.It prevents over loading any vehicle
    ![Objective](https://github.com/LanAnh55/Optimize/blob/main/contraints/constraint%203.png).
   
4+5. **Sub-tour Elimination** (Miller–Tucker–Zemlin):Prevent sub-tours that don’t include the depot by ensuring that the vehicle's load changes correctly between consecutive customer nodes.
   -The variables help track the cumulative load served, ensuring that the route is connected 𝑢 𝑖 and feasible.
   ![Objective](https://github.com/LanAnh55/Optimize/blob/main/contraints/constraints%204.png).
   
6. **Distance Limit**: Ensure that the total distance traveled by each vehicle does not exceed the maximum allowed distance. It reflects the practical limit of how far a vehicle can travel during its route
   ![Objective](https://github.com/LanAnh55/Optimize/blob/main/contraints/Constraints%206.png).
   
8. **Time Limit**:The total time for each vehicle (including travel and service time) does not exceed the maximum allowed time. It ensures that the vehicle completes its route within the allowable working hours,respecting the time constraints imposed by regulations or operational requirements.
   ![Objective](https://github.com/LanAnh55/Optimize/blob/main/contraints/Constraints%207.png).

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

 ![Objective](https://github.com/LanAnh55/Optimize/blob/main/image/solution.png).

**Cost and time comparison chart**

 ![Objective](https://github.com/LanAnh55/Optimize/blob/main/image/estimate.png).

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
**Contact:** Nguyễn Thị Lan Anh ([lananh2004@gmail.com](lananh2004@gmail.com))
