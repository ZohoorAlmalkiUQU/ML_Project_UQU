# ABR_ML

This repository contains the code and experimental pipeline used in the study:

**"Comparative Evaluation of Ensemble Machine Learning Models for Predicting Antibacterial Resistance from Electronic Health Records"**

The project investigates the effectiveness of ensemble machine learning models for predicting antibacterial resistance using microbiological culture data and patient-level clinical variables extracted from electronic health records.

---

## Overview

The project is structured as a notebook-driven research pipeline. Based on the repository contents, it includes:

* patient-level sampling and dataset preparation
* feature merging and dataset construction
* exploratory data analysis
* main machine learning pipeline
* dataset statistics generation
* model outputs and evaluation artifacts
* SHAP-based explainability outputs

The repository currently includes the main notebooks `00_sampling.ipynb` through `05_dataset_statistics.ipynb`, along with output folders such as `results/`, `shap/`, `statistics/`, and `figs/`. It also contains raw/intermediate data directories including `new_sample_one/`, `new_sample_one_processed/`, and a DOI-linked dataset folder.

---

## Repository Structure

```text
ABR_ML/
├── 00_sampling.ipynb
├── 01_prepare_sample.ipynb
├── 02_merge_sample.ipynb
├── 03_EDA_sample.ipynb
├── 04_main_pipeline.ipynb
├── 05_dataset_statistics.ipynb
├── catboost_info/
├── doi_10_5061_dryad_jq2bvq8kp__v20250411/
├── figs/
├── new_sample_one/
├── new_sample_one_processed/
├── merged_sample_one_processed/
├── results/
├── shap/
├── statistics/
├── .gitignore
├── LICENSE
└── README.md
```

### Notebook summary

#### `00_sampling.ipynb`

Creates or extracts the working sample used for downstream modeling.

#### `01_prepare_sample.ipynb`

Cleans and prepares source tables and intermediate features.

#### `02_merge_sample.ipynb`

Merges processed tables into the modeling dataset.

#### `03_EDA_sample.ipynb`

Performs exploratory data analysis and inspection of the prepared sample.

#### `04_main_pipeline.ipynb`

Runs the core machine learning workflow, which may include:

* train/test splitting
* patient-aware validation strategy
* model training
* hyperparameter tuning
* calibration
* performance evaluation
* explainability analysis

#### `05_dataset_statistics.ipynb`

Generates descriptive statistics for the final dataset.

---

## Typical Workflow

Run the notebooks in order:

1. `00_sampling.ipynb`
2. `01_prepare_sample.ipynb`
3. `02_merge_sample.ipynb`
4. `03_EDA_sample.ipynb`
5. `04_main_pipeline.ipynb`
6. `05_dataset_statistics.ipynb`

This sequence reflects the current repository layout and notebook naming convention.

---

## Outputs

Depending on the notebook configuration, generated outputs may be saved in:

* `results/` — model metrics, predictions, summaries, and evaluation artifacts
* `shap/` — SHAP plots and feature importance outputs
* `statistics/` — descriptive statistics tables and related summaries
* `figs/` — manuscript-ready figures
* `catboost_info/` — CatBoost training logs and metadata

---

## Data

The repository contains directories used to store sampled and processed data generated during the workflow, as well as a folder for the downloaded ARMD dataset:

* `new_sample_one/`
* `new_sample_one_processed/`
* `merged_sample_one_processed/`
* `doi_10_5061_dryad_jq2bvq8kp__v20250411/`

Because this is a research repository, some datasets may be large, partially processed, de-identified, or subject to source-specific access conditions. Review the notebook code and directory contents carefully before reuse.

---
## Quick Start

Clone the repository:

```bash
git clone https://github.com/ZohoorAlmalkiUQU/ABR_ML.git
cd ABR_ML
```

## Environment Setup

Create a Python environment and install the required dependencies.

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -U pip
pip install -r requirements.txt
```

Then launch Jupyter:

```bash
jupyter notebook
```

---

## Reproducibility Notes

This repository is designed to support reproducible machine learning experiments for antimicrobial resistance prediction using the **Antibiotic Resistance Microbiology Dataset (ARMD)**.

Key implementation details are documented to ensure experimental transparency and reproducibility.

### Environment

- **Python version:** 3.11.14  
- **Package versions:** specified in `requirements.txt`  
- **Random seed:** 42 (used across dataset splitting, cross-validation, and model training)

### Dataset

The experiments use the **Antibiotic Resistance Microbiology Dataset (ARMD)**, a large de-identified dataset derived from electronic health records (EHR) from **Stanford Healthcare**.

The dataset integrates multiple clinical domains including:

- microbiological culture results
- organism identification
- antibiotic susceptibility testing
- patient demographics
- laboratory measurements
- vital signs
- prior medication exposure
- antibiotic exposure history
- comorbidity profiles

#### Data Linking

Records across dataset tables are linked using the following identifiers:

- `anon_id` – de-identified patient identifier  
- `pat_enc_csn_id_coded` – encounter identifier  
- `order_proc_id_coded` – microbiology culture order identifier  
- `order_time_jittered_utc` – jittered timestamp to preserve temporal ordering while protecting privacy

#### Dataset Access

The ARMD dataset is publicly available for research purposes through **Dryad**.

Due to dataset size and licensing restrictions, the raw data are **not included in this repository**.

Researchers can obtain the dataset from the official source.

#### Dataset Citation

If you use the dataset, please cite:

Nateghi Haredasht, F., et al.  
*Antibiotic Resistance Microbiology Dataset (ARMD).*  
Dryad Digital Repository.  
https://doi.org/10.5061/dryad.jq2bvq8kp

#### Source Tables

The following tables from the ARMD dataset were used as the primary data sources:

- `microbiology_cultures_cohort`
- `microbiology_cultures_prior_med`
- `microbiology_cultures_demographics`
- `microbiology_cultures_labs`
- `microbiology_cultures_vitals`
- `microbiology_cultures_antibiotic_class_exposure`
- `microbiology_cultures_comorbidity`

### Sampling Strategy

The original ARMD dataset contains microbiological culture records from over **283,000 adult patients**.  
To make model development computationally feasible while preserving meaningful variation in antimicrobial resistance outcomes, a patient-level sampling procedure was applied.

The sampling process was performed as follows:

1. A subset of patient identifiers (`anon_id`) was randomly selected using a fixed random seed (**seed = 42**).
2. All microbiological culture records corresponding to the selected patients were retrieved from the cohort dataset.
3. This approach ensures that **entire patient histories are preserved**, rather than sampling individual rows.

After sampling:

- **997 unique patients** were retained.
- All associated culture records for these patients were included in the working dataset.

Because multiple culture records may exist for the same patient, the resulting dataset contains **multiple observations per patient**.

#### Sampled Tables Used in This Repository

After the patient-level sampling step (`00_sampling.ipynb`), the following sampled tables were generated and used in the modeling pipeline:

- `cultures_cohort.parquet`
- `prior_med.parquet`
- `cultures_demographics.parquet`
- `cultures_labs.parquet`
- `cultures_vitals.parquet`
- `antibiotic_class_exposure.parquet`
- `cultures_comorbidity.parquet`

### Train/Test Split Strategy

To prevent information leakage:

- data are split **at the patient level**
- unique `anon_id` identifiers are used for splitting
- **80% of patients** are used for model development
- **20% of patients** are reserved for testing

This ensures that records from the same patient never appear in both training and test sets.

### Cross-Validation

Model development uses **GroupKFold cross-validation**, where:

- groups = patient identifiers (`anon_id`)
- each fold contains disjoint patient groups
- leakage between folds is prevented

### Missing Data Handling

In the original dataset:

- missing values are represented as **`null`**

Handling strategy:

- tree-based ensemble models (XGBoost, LightGBM, CatBoost, HistGradientBoosting) are used because they **handle missing values natively**
- no global imputation is applied unless baseline model explicitly noted in `04_main_pipeline.ipynb`

### Feature Inclusion Rules

Features were retained if they:

- were available **prior to or near the culture order time**
- represented clinically relevant variables such as laboratory results, vital signs, medication exposure, or comorbidities

### Feature Exclusion Rules

The following types of variables were removed to prevent leakage or redundancy:

- direct identifiers
- encounter identifiers
- timestamps
- very sparse features
- variables identified as non-informative during preprocessing (see notebook `01_prepare_sample.ipynb`)

### Probability Calibration

Final models undergo **probability calibration using isotonic regression** applied to cross-validation predictions.

### Threshold Selection

Classification thresholds are selected based on validation performance metrics to balance precision and recall for resistance prediction.

---

### Models Implemented
The following machine learning models are evaluated:

- XGBoost
- LightGBM
- CatBoost
- HistGradientBoostingClassifier

### Evaluation and Statistical Analysis

Model performance is evaluated using multiple complementary approaches assessing discrimination, calibration, and clinical utility.

#### Discrimination Metrics

- ROC–AUC
- PR–AUC

#### Threshold-Based Metrics

- F1 Score
- Precision
- Recall

#### Calibration

Calibration performance is evaluated using:

- Brier Score

Probability calibration is performed using **isotonic regression** applied to cross-validation predictions.

#### Statistical Comparison of Models

To assess whether performance differences between models are statistically significant:

- **DeLong test** is used to compare ROC–AUC values between models.
- **McNemar’s test** is used to compare paired classification errors between models on the test set.

#### Clinical Utility Analysis

To evaluate the potential clinical usefulness of the models, **Decision Curve Analysis (DCA)** is performed.  
This analysis estimates the **net benefit** of using each model across a range of decision thresholds and compares it against baseline strategies (treat-all and treat-none).

#### Uncertainty Estimation

Confidence intervals for key evaluation metrics are estimated using **bootstrap resampling**.

#### Model Explainability

Model interpretability is analyzed using **SHAP (SHapley Additive exPlanations)** to identify clinically relevant predictors of antibacterial resistance.

Confidence intervals are estimated using bootstrap resampling.

---

## Citation

If you use this repository, cite the associated manuscript or project repository once the formal paper citation is available.

```bibtex
@misc{almalki_abr_ml,
  author       = {Zohoor Almalki, Amjad Althagafi, Sarah Al-shareef},
  title        = {ABR_ML: Antibacterial Resistance Prediction},
  year         = {2026},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/ZohoorAlmalkiUQU/ABR_ML}}
}
```

---

## License

This repository is released under the **MIT License**.

---

## Contact

For questions or collaboration related to this repository, please contact me:

Zohoor Almalki. </br>
Master’s Student, Artificial Intelligence - Umm Al-Qura University. </br>
S44680217@uqu.edu.sa
