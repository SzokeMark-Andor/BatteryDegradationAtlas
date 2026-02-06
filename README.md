# Battery Degradation Atlas (BDA): Experimental Artifacts & Discovery Audit

**Date of Discovery Run:** 2026-01-31  
**License:** Apache 2.0  
**Associated Article:** *The Semantic Gap in Battery Prognostics: Quantifying the ’Thermodynamic Information Loss’ in Aggregated Datasets*

---

## 1. Overview
This repository contains the **reproducible code and evidence logs** for the Battery Degradation Atlas (BDA) project. It serves as the "hard evidence" backing the scientific findings regarding data schema heterogeneity in open-source battery datasets.

The artifacts document the automated scanning of **18 datasets** (including CALCE, NASA, and the Zenodo cluster) to identify "knee-point" degradation signatures.

> **Note:** The raw parquet data files are not included in this repository due to size constraints. The logs provided here (`/Evidence_Logs`) serve as the cryptographic proof of the run results.

---

## 2. Repository Structure

### 📂 `/Experimental_Code`
Contains the Python scripts used to execute the discovery pipeline and validation experiments.

| Script Name | Purpose |
| :--- | :--- |
| `run_v4_with_dataset_filter.py` | **The Main Engine.** Iterates through raw parquet files, validates schemas, extracts differential voltage metrics, and applies the discovery rule (`AbsRho >= 0.3`). |
| `experiment_B_schema_repair.py` | **Validation Experiment.** A targeted script that attempts to repair the failed `zenodo_matr` dataset by aliasing column names (`voltage_in_V` → `voltage_V`). Used to prove the "Negative Result" discussed in the paper. |
| `generate_gold_summary.py` | **Aggregator.** Compiles individual log files into the master `GOLD` text report. |

### 📂 `/Evidence_Logs`
Contains the immutable output logs from the 2026-01-31 run.

| File Name | Description |
| :--- | :--- |
| `gold_plus4_dataset_audit.csv` | **The Master Status Report.** A high-level table showing which of the 18 datasets passed or failed, and the primary error reason (e.g., `Schema Mismatch`). |
| `discovery_summary_*.csv` | **Cell-Level Results.** Detailed Spearman correlation scores for every single cell processed (e.g., `discovery_summary_calce.csv` shows high correlations, while `discovery_summary_zenodo_*.csv` shows failures). |
| `BDA_discovery_GOLD*.txt` | **Full Run Log.** A consolidated text trace of the entire discovery session. |

---

## 3. Key Findings Validated by These Artifacts

1.  **High-Fidelity Benchmarks:** The logs confirm that **CALCE** and **NASA** datasets produce strong degradation signals (Spearman $|\rho| \approx 0.85+$), validating the pipeline's extraction logic.
2.  **The Schema Barrier:** 12 Zenodo-hosted datasets failed systematically. The logs identify the root cause as a schema mismatch (non-standard column names like `voltage_in_V` instead of `voltage_V`).
3.  **Repair Complexity:** The `experiment_B_schema_repair.py` script demonstrates that simple column aliasing was insufficient to recover the `zenodo_matr` data, highlighting deeper structural inconsistencies in archival datasets.

---

## 4. Usage & Reproducibility

To run these scripts locally, you must have the BDA environment configured and the raw datasets downloaded to `../data/processed`.

### Environment Requirements
Ensure your Python environment (3.9+) has the necessary dependencies installed:
```bash
pip install pandas pyarrow numpy scipy
```

### Running the Discovery Pipeline
To reproduce the main scan of all 18 datasets:
```bash
python Experimental_Code/run_v4_with_dataset_filter.py
```

## 5. License

This project is licensed under the Apache License 2.0. You are free to use, modify, and distribute this software under the terms of the license. See the LICENSE file for full details.
