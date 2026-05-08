# Battery Degradation Atlas (BDA): Public Data and Code Package

**Public package version:** 2026-05  
**License:** Apache-2.0  
**Associated article:** *Impact of Data Representation and Protocol Metadata on Battery Prognostic Signals*

---

## 1. Overview

This repository contains the public, reproducibility-focused data and code package for the Battery Degradation Atlas (BDA) study on data representation, protocol metadata, and battery prognostic signals.

The package supports the article-level analyses showing that:

- flattening or aggregation of battery cycling records can weaken feature--fade associations;
- explicit protocol metadata can preserve rest/reference alignment information;
- raw rest-descriptor associations are partly aligned with aging and protocol sequence;
- preserved step metadata provides incremental held-out-cell prognostic value when evaluated with conservative controls.

This repository is intentionally limited to public scientific artifacts: processed analysis tables, reproducibility scripts, generated figures, package manifests, and data-availability notes. It does **not** include reviewer correspondence, journal submission files, Overleaf packages, raw third-party MAT/HDF5 files, or machine-specific temporary outputs.

---

## 2. Repository Contents

```text
.
├── README.md
├── RUN_REPRODUCIBILITY.md
├── DATA_AVAILABILITY.md
├── CITATION.cff
├── requirements.txt
├── .gitignore
├── data/
│   ├── stage2E_step_comment_extraction/
│   ├── stage2F_rest_reference_alignment/
│   ├── stage2G_deconfounding/
│   └── protocol_metadata_validation/
├── figures/
│   ├── main_article/
│   └── supplementary/
├── scripts/
├── raw_data/
│   └── README_raw_data_not_included.md
└── metadata/
    ├── PACKAGE_MANIFEST_SHA256.csv
    └── SHA256SUMS.txt
```

---

## 3. Included Public Artifacts

### 3.1 `data/stage2E_step_comment_extraction/`

Processed outputs from NASA randomized-use step-comment parsing. These files document the extraction of rest-prior-reference events, reference-discharge descriptors, charge-after-random-walk descriptors, comment counts, and MAT-file inventory summaries.

### 3.2 `data/stage2F_rest_reference_alignment/`

Rest/reference alignment outputs used to pair explicit rest-prior-reference windows with adjacent reference-discharge capacity benchmarks. This stage provides the core aligned analysis table used for protocol-aware SOH analysis.

Key public outputs include:

- `stage2F_rest_reference_capacity_analysis_table.csv`
- `stage2F_adjacent_rest_reference_pairs.csv`
- `stage2F_cell_level_summary.csv`
- `stage2F_rest_capacity_correlations.csv`
- `stage2F_leave_cell_out_prediction_metrics.csv`

### 3.3 `data/stage2G_deconfounding/`

Deconfounding outputs used to separate raw monotonic descriptor--SOH association from local residual and first-difference evidence.

Key public outputs include:

- `stage2G_v2_deconfounded_correlations.csv`
- `stage2G_v2_delta_summary_vs_age_baseline.csv`
- `stage2G_v2_leave_one_cell_out_prediction_summary.csv`
- `stage2G_v2_interpretation_flags.csv`
- `stage2G_v2_condition_group_summary.csv`

### 3.4 `data/protocol_metadata_validation/`

Protocol-metadata validation outputs testing whether preserved rest/pre-reference descriptors improve held-out-cell SOH prediction beyond conservative sequence and condition baselines.

Key public outputs include:

- `stage2O_delta_summary_vs_sequence_condition.csv`
- `stage2O_prediction_summary.csv`
- `stage2O_permutation_null_summary.csv`
- `stage2O_permutation_null_distribution.csv`
- `stage2O_leave_one_condition_out_summary.csv`
- `stage2O_sequence_residual_degree_sensitivity.csv`
- `stage2O_interpretation_flags.csv`

### 3.5 `scripts/`

Reproducibility scripts corresponding to the public analysis stages:

- `stage2E_v3_step_comment_extraction.py`
- `stage2F_rest_reference_alignment.py`
- `stage2G_v2_deconfounding.py`
- `stage2O_protocol_metadata_validation.py`

### 3.6 `figures/`

Article-level and supplementary figures generated from processed analysis outputs.

Main article figures include:

- `fig_stage2J_protocol_evidence.pdf`
- `fig_stage2J_deconfounded_validation.pdf`
- `fig_protocol_metadata_robustness_validation.pdf`

Supplementary figures include rest/reference trajectories, descriptor-vs-SOH plots, condition summaries, raw/deconfounded correlations, and prediction-delta summaries.

### 3.7 `metadata/`

Package integrity files:

- `SHA256SUMS.txt`
- `PACKAGE_MANIFEST_SHA256.csv`

These allow users to verify that the public package contents have not changed after packaging.

---

## 4. What Is Not Included

The following materials are intentionally excluded from the public GitHub repository:

- raw NASA/CALCE/third-party MAT, HDF5, ZIP, or large binary files;
- reviewer response documents;
- journal decision letters or private correspondence;
- Overleaf submission packages;
- blinded or unblinded manuscript source files;
- local cache files, temporary build outputs, and failed package archives;
- machine-specific absolute paths.

Raw datasets should be downloaded from their original public sources according to the instructions in `DATA_AVAILABILITY.md` and `raw_data/README_raw_data_not_included.md`.

---

## 5. Main Findings Supported by This Package

### 5.1 Data representation and flattening penalty

The CALCE comparison shows that flattening/aggregation can weaken feature--fade association. The structured representation preserves a stronger feature--fade association than the flattened representation.

### 5.2 Explicit protocol metadata enables rest/reference alignment

NASA randomized-use step comments enable alignment of explicit rest-prior-reference-discharge events with adjacent reference-discharge capacity benchmarks. The Stage 2F outputs document:

- 294 clean adjacent rest/reference pairs;
- 16 randomized-use cells;
- paired rest, charge, reference-discharge, and SOH descriptors.

### 5.3 Raw rest-descriptor association is not sufficient as a mechanism claim

The Stage 2G deconfounding outputs show that raw rest-descriptor association is strongly monotonic, but much of the association is aligned with aging and protocol sequence. Residual and first-difference checks are therefore interpreted conservatively.

The public interpretation guardrail is:

> Rest/pre-reference descriptors are treated as operational protocol metadata and prognostic alignment signals, not as direct evidence for a unique electrochemical degradation mechanism.

### 5.4 Preserved metadata provides incremental prognostic value

The Stage 2O validation outputs show that adding rest/pre-reference metadata to a sequence-plus-condition baseline improves held-out-cell SOH prediction under conservative controls.

The public validation package includes:

- leave-one-cell-out prediction summaries;
- noise and shuffled-descriptor controls;
- permutation-null testing;
- alternative SOH normalization;
- leave-one-condition-out sanity checks;
- residual descriptor sensitivity checks.

---

## 6. Reproducibility

### 6.1 Create the Python environment

Python 3.9+ is recommended.

```bash
python -m venv .venv
source .venv/bin/activate      # Linux/macOS
# .venv\Scripts\Activate.ps1  # Windows PowerShell

pip install -r requirements.txt
```

If `requirements.txt` is unavailable in a copied subset, install the core dependencies manually:

```bash
pip install pandas numpy scipy scikit-learn matplotlib pyarrow
```

### 6.2 Run the public analysis stages

The public scripts are provided for transparent inspection and rerun workflows. Some stages require locally downloaded raw source data, which are not redistributed in this repository.

```bash
python scripts/stage2E_v3_step_comment_extraction.py
python scripts/stage2F_rest_reference_alignment.py
python scripts/stage2G_v2_deconfounding.py
python scripts/stage2O_protocol_metadata_validation.py
```

For a detailed rerun guide, see:

```text
RUN_REPRODUCIBILITY.md
DATA_AVAILABILITY.md
raw_data/README_raw_data_not_included.md
```

---

## 7. Data Availability

This repository redistributes only processed, lightweight, article-supporting outputs. Raw battery datasets remain with their original publishers and must be obtained from the original public dataset sources.

This design avoids duplicating large third-party raw archives and keeps the GitHub repository focused on reproducible analysis artifacts.

---

## 8. Integrity Checks

To verify package integrity, compare file hashes against:

```text
metadata/SHA256SUMS.txt
metadata/PACKAGE_MANIFEST_SHA256.csv
```

Example check on Linux/macOS:

```bash
sha256sum -c metadata/SHA256SUMS.txt
```

On Windows PowerShell, individual files can be checked with:

```powershell
Get-FileHash path\to\file -Algorithm SHA256
```

---

## 9. Citation

If you use this package, please cite the associated article and the repository release.

A machine-readable citation file is provided:

```text
CITATION.cff
```

Suggested repository citation format:

```text
Szoke, M. Battery Degradation Atlas (BDA): Public Data and Code Package.
GitHub repository, 2026.
```

Update the citation with the final article DOI and repository release DOI if/when available.

---

## 10. License

This repository is intended to be released under the Apache License 2.0.

Before making the repository public, ensure that the root folder contains a valid license file:

```text
LICENSE
```

or:

```text
LICENSE.md
```

The license applies to the code and repository-authored documentation. Third-party raw datasets are not redistributed here and remain governed by their original data-source terms.

---

## 11. Maintainer Notes

Recommended public upload:

- keep this README as the root `README.md`;
- keep `CITATION.cff`, `DATA_AVAILABILITY.md`, `RUN_REPRODUCIBILITY.md`, and `requirements.txt`;
- keep the `data/`, `figures/`, `scripts/`, `metadata/`, and `raw_data/` folders from the clean public package;
- do not upload journal-response documents, reviewer letters, Overleaf ZIPs, blinded/unblinded manuscripts, or private correspondence;
- do not upload raw MAT/HDF5 archives unless using an external archival repository or Git LFS with clear data rights.

Older January 2026 audit logs may remain in a separate legacy folder or release archive, but this README describes the cleaned public Stage 2 package used for the final article-level evidence.
