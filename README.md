# Comorbidity-Aware Inductive Biases for Multi-Label Clinical Prediction

This repository contains the code, results, and analysis for an empirical
evaluation of comorbidity-aware inductive biases for multi-label prediction
across two clinical domains: Type 2 Diabetes complications and Myocardial Infarction electrical complications.

## Overview

Multi-label clinical prediction models routinely treat correlated outcomes as
independent. This work investigates whether explicitly encoding comorbidity
structure into the output architecture provides a consistent advantage over a
well-matched multi-task baseline. Four interaction mechanisms were evaluated,
each encoding a different structural assumption about how outcomes relate.

**Key finding:** The symmetric Conditional Random Field (CRF), which assumes
complications co-occur without directionality due to a shared upstream biological
cause, was the only mechanism to improve consistently over the baseline at every
data fraction tested — using only 3 learnable parameters.

## Outcomes Predicted

**Study 1: Type 2 Diabetes Complications (T2D)**
- NEP: Nephropathy (kidney disease)
- NEU: Neuropathy (nerve damage)
- RET: Retinopathy (eye disease)

**Study 2: Myocardial Infarction Complications (MI)**
- VT: Ventricular Tachycardia
- VF: Ventricular Fibrillation
- AV Block: Atrioventricular Block

## Interaction Mechanisms Tested

| # | Mechanism | Extra Parameters | Structural Assumption |
|---|-----------|-----------------|----------------------|
| 1 | Linear Additive | 6 | Fixed additive influence |
| 2 | Multiplicative | 6 | Synergistic scaling |
| 3 | CRF (Symmetric) | 3 | Undirected co-occurrence |
| 4 | Residual MLP | 123 | Unconstrained (upper bound) |

## Repository Structure
```text
comorbidity-inductive-bias/
├── data/                    # Open-sourced datasets
│   ├── t2d_data.csv
│   └── MI.data
├── notebooks/               
│   ├── t2d/                 # T2D experiments
│   │   ├── depth_experiment.ipynb
│   │   ├── linear_additive.ipynb
│   │   ├── multiplicative.ipynb
│   │   ├── crf.ipynb
│   │   ├── residual_mlp.ipynb
│   │   └── combined_head.ipynb
│   └── mi/                  # MI experiments
│       ├── baseline.ipynb
│       ├── linear_additive.ipynb
│       ├── multiplicative.ipynb
│       └── crf.ipynb
├── results/                 # Master results JSON per experiment
│   ├── t2d/
│   └── mi/
└── requirements.txt
```

## How to Run

1. Clone the repository
2. Install dependencies: `pip install -r requirements.txt`
3. Ensure datasets are present in the `data/` directory.
4. Navigate to `notebooks/t2d/` or `notebooks/mi/` and run the experiment notebooks in any order.

## Publication Figures

*Note: The publication figures (e.g., the `.tif` line plots and heatmaps) are not statically hosted or generated in this repository. All figures were implemented and compiled natively via TikZ/PGFPlots directly within the LaTeX manuscript (`main.tex`) using the raw data aggregated in the `results/` folder.*

## Experimental Design

- **Seeds:** 7 independent runs per condition (seeds 42, 123, 456, 789, 1337, 2024, 9999)
- **Data fractions:** 100%, 80%, 60%, 40%, 20% of training set
- **Evaluation:** Macro AUROC on fixed held-out test set

## Data

### Type 2 Diabetes Dataset
The dataset used in this project contains clinical records of patients diagnosed
with Type 2 diabetes. It is open-sourced and can be accessed from Mendeley Data.

**Citation:**
Vamsi, Bandi; Bhattacharyya, Debnath (2021), “Micro and Macro vascular complications in Type_II diabetes”, Mendeley Data, V1, doi: 10.17632/dsjcb6pyd8.1

For access to the dataset, please download it from Mendeley Data using the DOI: [10.17632/dsjcb6pyd8.1](https://doi.org/10.17632/dsjcb6pyd8.1) and place the CSV file at `data/t2d_data.csv`.

### Myocardial Infarction Complications Dataset
The MI dataset contains records of patients with acute myocardial infarction. The dataset includes 124 features encompassing demographics, history, admission vitals, and ECG findings.

**Citation:**
Golovenkin, S. E., Bac, J., Chervov, A., Mirkes, E. M., Orlova, Y., Barillot, E., Gorban, A., & Zinovyev, A. (2020). Trajectories, bifurcations, and pseudo-time in large clinical datasets: applications to myocardial infarction and diabetes data. GigaScience.

For access to the dataset, please download it from the UCI Machine Learning Repository (ID: 579) and place the data file at `data/MI.data`.

## Author

Asumboya Wilfred Ayine
BSc. Biomedical Engineering L300, University of Ghana
Independent Research, 2026
