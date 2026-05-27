# Cold Chain Supply Chain Optimisation

An integrated operations research model for pharmaceutical cold chain management, combining EOQ inventory optimisation, LP-based transportation routing, Monte Carlo demand simulation, and scipy-based integrated optimisation.

---

## Overview

Cold chain logistics — the temperature-controlled distribution of vaccines, biologics, and oncology drugs — is one of the most cost-sensitive and operationally complex areas of supply chain management. This project builds a **multi-model optimisation framework** for a pharmaceutical cold chain network across India, covering 5 warehouses and 6 regional customer zones for 5 product types.

The model integrates four distinct operations research techniques into a single pipeline, with results exported to a structured Excel dashboard.

---

## Problem Setting

| Dimension | Details |
|-----------|---------|
| Products | 5 pharmaceutical products — mRNA vaccine (−80°C), influenza vaccine, insulin, adalimumab (Humira), trastuzumab (Herceptin) |
| Warehouses | 5 cold hubs — Mumbai, Delhi, Bengaluru, Chennai, Kolkata |
| Customer Regions | 6 zones — North, South, East, West, Central, Northeast India |
| Objective | Minimise total cost: inventory holding + ordering + shortage + transportation |

---

## Approach

### Part 1 — Data Generation
Synthetic but realistic product catalogue, warehouse capacities, regional demand parameters, and a distance matrix with temperature-based transport cost surcharges.

### Part 2 — EOQ & Inventory Optimisation
- **Economic Order Quantity (EOQ)** for each product–customer pair
- **Safety stock** with stochastic lead time (normal demand, variable lead time)
- **Reorder point** calculation
- **Newsvendor model** for seasonal vaccine products
- Total inventory cost (holding + ordering) per SKU

### Part 3 — Transportation Problem (Linear Programming)
- Balanced transportation LP using `scipy.optimize.linprog` (HiGHS solver)
- Supply from warehouses allocated proportionally by capacity
- Demand set to monthly regional requirements
- Dummy supply/demand added automatically to balance the problem
- Optimal shipment plan generated for each of the 5 products

### Part 4 — Monte Carlo Demand Simulation
- 500 simulations × 90-day horizon per product
- Stochastic daily demand (normal distribution)
- Temperature excursion events modelled probabilistically (higher risk for ultra-cold products)
- Outputs: service level, stockout probability, average holding/shortage/total cost

### Part 5 — Integrated Optimisation
- `scipy.optimize.minimize` (L-BFGS-B) over safety stock and EOQ multipliers
- Objective: minimise combined holding + ordering + shortage cost
- Bounds: multipliers constrained to [0.5, 2.0]

### Part 6 — Visualisation Dashboard
6-panel matplotlib dashboard covering EOQ by product/customer, transportation heatmap, simulated service levels, cost breakdown, safety stock vs reorder point scatter, and route-level transport costs.

---

## Results

- Full EOQ, safety stock, and reorder point table for all 30 product–customer combinations
- Optimal transportation plan for each product (₹ total transport cost reported)
- Simulated service levels and stockout probabilities per product
- Results exported to `cold_chain_optimization_results.xlsx` with separate sheets per analysis

---

## Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-Optimisation-8CAAE6?style=flat-square&logo=scipy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat-square&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-4C8CBF?style=flat-square&logoColor=white)
![OpenPyXL](https://img.shields.io/badge/OpenPyXL-Excel_Export-217346?style=flat-square&logoColor=white)

---

## Repository Structure

```
cold-chain-supply-chain/
│
├── code.ipynb                            # Full integrated model notebook
├── cold_chain_optimization_results.xlsx  # Exported results (EOQ, transport, simulation)
└── cold_chain_final_report.docx          # Written report
```

---

## Key Concepts Demonstrated

- Economic Order Quantity (EOQ) with stochastic lead time
- Newsvendor model for perishable/seasonal products
- Balanced transportation problem via Linear Programming
- Monte Carlo simulation for inventory risk analysis
- Integrated cost optimisation with scipy minimiser
- Cold chain constraints — temperature excursion risk, shelf life

---

*Part of coursework in Operations and Business Decision Analytics (OBDA) — PGDBA, IIM Calcutta · IIT Kharagpur · ISI Kolkata*
