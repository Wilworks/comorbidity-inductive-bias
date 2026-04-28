# Comorbidity-Aware Inductive Biases for Multi-Label Clinical Prediction

This repository contains the code, results, and analysis for an empirical
evaluation of comorbidity-aware inductive biases for multi-label prediction
of Type 2 diabetes complications.

## Overview

Multi-label clinical prediction models routinely treat correlated outcomes as
independent. This work investigates whether explicitly encoding comorbidity
structure into the output architecture provides a consistent advantage over a
well-matched multi-task baseline. Six interaction mechanisms were evaluated,
each encoding a different structural assumption about how outcomes relate.

**Key finding:** The symmetric Conditional Random Field (CRF), which assumes
complications co-occur without directionality due to a shared upstream biological
cause, was the only mechanism to improve consistently over the baseline at every
data fraction tested — using only 3 learnable parameters.

## Outcomes Predicted

- NEP: Nephropathy (kidney disease)
- NEU: Neuropathy (nerve damage)
- RET: Retinopathy (eye disease)

## Interaction Mechanisms Tested

| # | Mechanism | Extra Parameters | Structural Assumption |
|---|-----------|-----------------|----------------------|
| 1 | Linear Additive | 6 | Fixed additive influence |
| 2 | Multiplicative | 6 | Synergistic scaling |
| 3 | Low-Rank (R=1) | 6 | Single latent comorbidity factor |
| 4 | CRF (Symmetric) | 3 | Undirected co-occurrence |
| 5 | Bilinear Attention | 54 | Patient-specific interaction weights |
| 6 | Residual MLP | 123 | Unconstrained (upper bound) |

## Repository Structure
comorbidity-inductive-bias/
├── data/                    # Open-sourced dataset (see Data section)
├── notebooks/               # One notebook per experiment
│   ├── 01_preprocessing.ipynb
│   ├── 02_depth_experiment.ipynb
│   ├── 03_linear_additive.ipynb
│   ├── 04_multiplicative.ipynb
│   ├── 05_lowrank.ipynb
│   ├── 06_crf.ipynb
│   ├── 07_attention.ipynb
│   ├── 08_residual_mlp.ipynb
│   └── 09_analysis.ipynb
├── plots/                   # All generated figures
├── results/                 # Master results JSON per experiment
└── requirements.txt


## How to Run

1. Clone the repository
2. Install dependencies: `pip install -r requirements.txt`
3. Place your dataset at `data/data.csv` (see Data section for expected format)
4. Run notebooks in order starting from `01_preprocessing.ipynb`
5. Run `09_analysis.ipynb` after all experiments are complete to generate plots

## Experimental Design

- **Seeds:** 3 independent runs per condition (seeds 42, 123, 456)
- **Data fractions:** 100%, 80%, 60%, 40%, 20% of training set
- **Backbone:** 3 hidden layers, hidden_dim=16, proj_dim=8 (~1,107 parameters)
- **Evaluation:** Macro AUROC on fixed held-out test set
- **Total training runs:** 180

## Data

The dataset used in this project contains clinical records of patients diagnosed
with Type 2 diabetes. It is open-sourced and can be accessed from Mendeley Data.

**Citation:**
Vamsi, Bandi; Bhattacharyya, Debnath (2021), “Micro and Macro vascular complications in Type_II diabetes”, Mendeley Data, V1, doi: 10.17632/dsjcb6pyd8.1

### Dataset Structure

The dataset consists of 3,069 records and 22 columns. The expected columns are:

| Column | Description |
|--------|-------------|
| SL.NO | Serial number (excluded from features) |
| NAME | Patient name (excluded from features) |
| AGE | Patient age in years |
| SEX | Patient sex (binary encoded) |
| BMI | Body mass index |
| SP | Systolic blood pressure |
| BP | Diastolic blood pressure |
| HbA1c | Glycated haemoglobin (%) |
| FPS | Fasting plasma sugar (mg/dL) |
| PPS | Postprandial plasma sugar (mg/dL) |
| FAMILY H/O | Family history of diabetes (binary) |
| ONSET AGE | Age of diabetes onset |
| DIA LIFE | Diabetes duration (years or months — see preprocessing) |
| SMOKING | Smoking status (binary) |
| PHY ACT | Physical activity level (binary) |
| MED USE | Medication use (binary) |
| MED ADH | Medication adherence (binary) |
| NEP | Nephropathy — target outcome (binary) |
| NEU | Neuropathy — target outcome (binary) |
| RET | Retinopathy — target outcome (binary) |
| CV | Cardiovascular disease (used as feature) |
| PER VAS | Peripheral vascular disease (used as feature) |

### Preprocessing Note

The DIA LIFE column contains mixed formats — some values are in years
(e.g., 5, 12) and some in months using both the word "month" and the
abbreviation "m" (e.g., "9 m", "11 month"). The preprocessing notebook
handles both formats by converting month values to years.

For access to the dataset, please download it from Mendeley Data using the DOI: [10.17632/dsjcb6pyd8.1](https://doi.org/10.17632/dsjcb6pyd8.1) and place the CSV file at `data/data.csv`.

## Author

Asumboya Wilfred Ayine
BSc. Biomedical Engineering L300, University of Ghana
Independent Research, 2026
