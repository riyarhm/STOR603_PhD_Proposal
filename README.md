# Uncertainty Quantification in Traffic Performance Statistics

**A Multiple Imputation Approach for Missing Road Sensor Data**

This repository contains the data generation, imputation, and analysis code for my Master of Research dissertation at Lancaster University (STOR-i Centre for Doctoral Training), conducted in partnership with National Highways.

## Overview
Missing road sensor data degrades the reliability of traffic performance statistics, such as Total Miles Travelled (TMT). Traditional deterministic infilling methods, like Stratified Historical Average (SHA), fail to account for the uncertainty introduced by data reconstruction. 

This project implements a simulation-based multiple imputation framework to quantify missing data uncertainty. It compares SHA against Multivariate Imputation by Chained Equations (MICE) using Predictive Mean Matching (MICE-PMM) and Random Forests (MICE-RF) across three controlled missingness scenarios:
* **Pattern A:** Scattered random data loss (MCAR)
* **Pattern B:** Sustained multivariate hardware outages (Block MAR)
* **Pattern C:** Atypical non-recurring congestion shocks (with and without historical donors)

## Repository Structure

The workflow is divided into three sequential stages:

```text
STOR603-PhD-Proposal/
├── data_generation/
│   ├── groundtruth_no_history.ipynb    # Generates 90-day base synthetic traffic (No historic shocks)
│   ├── groundtruth_with_history.ipynb  # Generates 90-day base synthetic traffic (With historic shocks)
│   ├── generate_pattern_A.ipynb        # Injects random scattered missingness
│   ├── generate_pattern_B.ipynb        # Injects continuous sensor hardware failure
│   └── generate_pattern_C.ipynb       # Injects missingness during a congestion shock (Case 1 and 2)
├── imputation_algorithms/
│   ├── Pattern_A.Rmd            # Applies SHA, MICE-PMM, and MICE-RF to Pattern A
│   ├── Pattern_B.Rmd            # Applies SHA, MICE-PMM, and MICE-RF to Pattern B
│   ├── Pattern_C1.Rmd           # Applies SHA, MICE-PMM, and MICE-RF to Pattern C1
│   └── Pattern_C2.Rmd           # Applies SHA, MICE-PMM, and MICE-RF to Pattern C2
├── performance_analysis/
│   ├── pattern_A_analysis.ipynb        # Cell-level and TMT evaluation for Pattern A
│   ├── pattern_B_analysis.ipynb        # Cell-level and TMT evaluation for Pattern B
│   ├── pattern_C1_analysis.ipynb       # Cell-level and TMT evaluation for Pattern C1
│   ├── pattern_C2_analysis.ipynb       # Cell-level and TMT evaluation for Pattern C2
│   └── macroanalysis_sensitivity.ipynb# 10-seed sensitivity and interval coverage evaluation
├── LICENSE 
└── README.md
```

#### Note on Code Architecture:
To facilitate code review, please note that the core algorithmic implementations for the SHA, MICE-PMM, and MICE-RF methods are identical across all three experimental cases. While the individual execution scripts are extensive in length, the variations between them are limited exclusively to the specification of target variables and the selection of predictor variables required for each specific missingness pattern.

## Prerequisites

This pipeline requires both Python and R. 

**Python Dependencies (Data Generation & Analysis):**
* `pandas`
* `numpy`
* `scipy`
* `os`
* `matplotlib` / `seaborn` (for visualizations)
* `jupyter`

**R Dependencies (Imputation):**
* `mice`
* `randomForest`
* `dplyr`
* `readr`
* `lubridate`
* `tibble`

## Execution Pipeline

To reproduce the findings in the dissertation, run the files in the following order:

1. **Data Generation:** Run the notebooks in `data_generation/` to create the complete synthetic groundtruth datasets and inject the specific missingness patterns. *(Note: Data files are excluded from this repository).*
2. **Imputation:** Run the R Markdown scripts in `imputation_algorithms/`. These scripts ingest the incomplete datasets, run the multiple imputation algorithms (m=5), and output the completed datasets.
3. **Analysis:** Run the notebooks in `analysis/` to evaluate local cell-level reconstruction accuracy (RMSE, MAE) and apply Rubin's Rules to pool the network-level Total Miles Travelled (TMT) estimates.
