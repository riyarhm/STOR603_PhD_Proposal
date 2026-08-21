# Uncertainty Quantification in Traffic Performance Statistics

**A Multiple Imputation Approach for Missing Road Sensor Data**

This repository contains the data generation, imputation, and analysis code for my Master of Research dissertation at Lancaster University (STOR-i Centre for Doctoral Training), conducted in partnership with National Highways.

## Overview
Missing road sensor data degrades the reliability of traffic performance statistics, such as Total Miles Travelled (TMT). Traditional deterministic infilling methods, like Stratified Historical Average (SHA), fail to account for the uncertainty introduced by data reconstruction. 

This project implements a simulation-based multiple-imputation framework to quantify missing-data uncertainty. It compares SHA against Multivariate Imputation by Chained Equations (MICE) using Predictive Mean Matching (MICE-PMM) and Random Forests (MICE-RF) across three controlled missingness scenarios:
* **Pattern A:** Scattered random data loss (MCAR)
* **Pattern B:** Sustained multivariate hardware outages (Block MAR)
* **Pattern C:** Atypical non-recurring congestion shocks (with and without historical donors)

## Repository Structure

The workflow is divided into three sequential stages:

```text
StTOR603-PhD-Proposal/
├── data_generation/
│   ├── groundtruth_no_history.ipynb    # Generates 90-day base synthetic traffic (No historic shocks)
│   ├── groundtruth_with_history.ipynb  # Generates 90-day base synthetic traffic (With historic shocks)
│   ├── generate_pattern_A.ipynb        # Injects random scattered missingness
│   ├── generate_pattern_B.ipynb        # Injects continuous sensor hardware failure
│   └── generate_pattern_C2.ipynb       # Injects missingness during a congestion shock (Case 1 and 2)
├── 02_imputation_algorithms/
│   ├── impute_pattern_A.Rmd            # Applies SHA, MICE-PMM, and MICE-RF to Pattern A
│   ├── impute_pattern_B.Rmd            # Applies SHA, MICE-PMM, and MICE-RF to Pattern B
│   ├── impute_pattern_C1.Rmd           # Applies SHA, MICE-PMM, and MICE-RF to Pattern C1
│   └── impute_pattern_C2.Rmd           # Applies SHA, MICE-PMM, and MICE-RF to Pattern C2
├── 03_analysis/
│   ├── analysis_pattern_A.ipynb        # Cell-level and TMT evaluation for Pattern A
│   ├── analysis_pattern_B.ipynb        # Cell-level and TMT evaluation for Pattern B
│   ├── analysis_pattern_C1.ipynb       # Cell-level and TMT evaluation for Pattern C1
│   ├── analysis_pattern_C2.ipynb       # Cell-level and TMT evaluation for Pattern C2
│   └── macro_analysis_sensitivity.ipynb# 10-seed sensitivity and interval coverage evaluation
├── README.md
└── LICENSE
```
## Prerequisites

This pipeline requires both Python and R. 

**Python Dependencies (Data Generation & Analysis):**
* `pandas`
* `numpy`
* `scipy`
* `matplotlib` / `seaborn` (for visualizations)
* `jupyter`

**R Dependencies (Imputation):**
* `mice`
* `randomForest`
* `dplyr`

## Execution Pipeline

To reproduce the findings in the dissertation, run the files in the following order:

1. **Data Generation:** Run the notebooks in `data_generation/` to create the complete synthetic groundtruth datasets and inject the specific missingness patterns. *(Note: Data files are excluded from this repository).*
2. **Imputation:** Run the R Markdown scripts in `imputation_algorithms/`. These scripts ingest the incomplete datasets, run the multiple imputation algorithms (m=5), and output the completed datasets.
3. **Analysis:** Run the notebooks in `analysis/` to evaluate local cell-level reconstruction accuracy (RMSE, MAE) and apply Rubin's Rules to pool the network-level Total Miles Travelled (TMT) estimates.
