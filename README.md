# Optimize
Title: Multiple-Depot Vehicle Routing Problem with Logistics  Chain Optimization and Scheduling of Hospital Pharmacy Drugs

# Abstract

This project focuses on solving a real-world logistics challenge in the healthcare sector: the distribution and scheduling of hospital pharmacy drugs using the Multiple-Depot Vehicle Routing Problem (MDVRP). The objective was to design a solution approach that optimizes vehicle routing for centralized pharmacies serving multiple hospitals in Morocco. The problem is formulated as a variant of the classical VRP, taking into account realistic constraints such as vehicle capacity, drug demand, service time, maximum travel distance, and delivery time windows.

To address this, we developed a mathematical model using Mixed Integer Linear Programming (MILP), implemented with IBM ILOG CPLEX Optimization Studio via the DOCPLEX library in Python. The model is supported by preprocessing (e.g., distance matrix generation, constraint setup) and post-processing (e.g., route visualization, capacity monitoring) components. A key component of this project was the simulation of real hospital delivery scenarios, with results demonstrating optimal solutions (e.g., 4015 km minimum cost) within feasible computational time. A graphical user interface was also developed to visualize delivery paths and analyze vehicle efficiency.

This work provides a foundation for future enhancements, including the integration of real-time data, reverse logistics, and environmental constraints, aiming to improve decision-making efficiency and public health outcomes.
# Key work 
Vehicle Routing Problem(VRPB); Open vehicle routing problem; Lagrangian decomposition; Lagrangian relaxation algorithm; Clustering algorithm; CPLEX optimization solver; Python implementation
