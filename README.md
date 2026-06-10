# Comorbidity-Aware Inductive Biases for Multi-Label Clinical Prediction

This repository contains the code, results, and analysis for an empirical
evaluation of comorbidity-aware inductive biases for multi-label prediction
across two clinical domains: Type 2 Diabetes complications and Myocardial Infarction electrical complications.

## Overview

Multi-label clinical prediction models routinely treat correlated outcomes as
independent. This work investigates whether explicitly encoding comorbidity
structure into the output architecture provides a consistent advantage over a
well-matched multi-task baseline. Six output-head designs were evaluated in T2D, and four in MI,
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

## Output-Head Designs Tested

| Mechanism | Extra Parameters | Structural Assumption | Evaluated In |
|-----------|------------------|-----------------------|--------------|
| Independent Baseline | 0 | Conditionally independent | T2D & MI |
| Linear Additive | 6 | Fixed additive influence | T2D & MI |
| Multiplicative | 6 | Synergistic scaling | T2D & MI |
| CRF (Symmetric) | 3 | Undirected co-occurrence | T2D & MI |
| Residual MLP | 123 | Unconstrained (upper bound) | T2D Only |
| Combined Additive-Multiplicative | 12 | Additive + Synergistic scaling | T2D Only |

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

## Reproducibility Guide

The codebase is designed to allow full reproduction of the 350 training runs.

### 1. Environment Setup
Clone the repository and install the required dependencies:
```bash
git clone https://github.com/your-username/comorbidity-inductive-bias.git
cd comorbidity-inductive-bias
pip install -r requirements.txt
```

### 2. Dataset Preparation
Ensure both the T2D and MI datasets are downloaded and placed in the `data/` directory as described in the **Data** section below.

### 3. Path Configuration (Colab vs. Local)
All experiments were originally executed using **Google Colab**. Consequently, the data loading and result saving paths in the top cell of each notebook point to Google Drive directories (e.g., `BASE_DRIVE = '/content/drive/MyDrive/comorbidity_experiments/'`).

- **To run in Google Colab:** Upload the `data/` directory to your Google Drive and ensure the `BASE_DRIVE` variable in the notebooks matches your Drive structure.
- **To run locally:** Open each notebook and modify the file paths in the first cell to point to your local directories. For example:
  - `DATA_PATH = '../../data/t2d_data.csv'`
  - `BASE_DRIVE = '../../results/t2d/'`

### 4. Running the Experiments
Navigate to `notebooks/t2d/` or `notebooks/mi/`. You can run the experiment notebooks (e.g., `baseline.ipynb`, `crf.ipynb`) in any order. Executing all cells in a notebook will train the model across the predefined 7 seeds and 5 data fractions, automatically appending the evaluated metrics to the corresponding `master_results.json` file in your results directory.

### 5. Reproducing the Final Paper Results (Figures)
The final results, figures (e.g., line plots, heatmaps), and tables presented in the publication are generated natively via TikZ/PGFPlots directly within the LaTeX manuscript (`main.tex`). 

The `results/` directory contains all the raw aggregate AUROC data in JSON format that the LaTeX manuscript relies upon. To reproduce the visual results exactly as they appear in the paper, you do not need to run an external Python analysis script; simply compile the manuscript source code using your preferred LaTeX distribution.

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
The MI dataset contains 1,700 records of patients with acute myocardial infarction. The official dataset includes 111 multivariate input features encompassing demographics, history, admission vitals, and ECG findings.

**Citation:**
Golovenkin SE, Shulman VA, Rossiev DA, Shesternya PA, Nikulina SY, Orlova YV, et al. Myocardial infarction complications [Dataset]. UCI Machine Learning Repository; 2020. doi:10.24432/C53P5M.

For access to the dataset, please download it from the UCI Machine Learning Repository (ID: 579) and place the data file at `data/MI.data`.

## Author

Wilfred Ayine
BSc. Biomedical Engineering L300, University of Ghana
Independent Research, 2026
