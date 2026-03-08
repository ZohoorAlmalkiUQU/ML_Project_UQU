# ABR_ML

Machine learning pipeline for **antibacterial resistance prediction** using **electronic health record (EHR)-derived microbiology data**.

This repository organizes the end-to-end workflow for sampling, preprocessing, merging, exploratory data analysis, model development, statistical evaluation, and explainability for an antibacterial resistance prediction study.

---

## Overview

The project is structured as a notebook-driven research pipeline. Based on the repository contents, it includes:

* sample creation and preparation
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

The repository includes local data-related folders for sampled and processed data, as well as a DOI-linked dataset directory:

* `new_sample_one/`
* `new_sample_one_processed/`
* `doi_10_5061_dryad_jq2bvq8kp__v20250411/`

Because this is a research repository, some datasets may be large, partially processed, de-identified, or subject to source-specific access conditions. Review the notebook code and directory contents carefully before reuse.

---

## Environment Setup

Create a Python environment and install the packages used in the notebooks.

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -U pip
pip install jupyter numpy pandas scipy scikit-learn matplotlib xgboost lightgbm catboost shap pyarrow fastparquet
```

Then launch Jupyter:

```bash
jupyter notebook
```

> It is a good idea to export a pinned `requirements.txt` or `environment.yml` from your working environment for exact reproducibility.

---

## Reproducibility Notes

To improve reproducibility, consider documenting the following in the codebase if not already included:

* Python version
* package versions
* random seeds
* exact train/test split strategy
* patient/group leakage prevention strategy
* handling of missing values
* feature inclusion/exclusion rules
* calibration and threshold selection procedures

---

## Suggested README Additions for Publication-Ready Use

For a stronger public research repository, you may also want to add:

* a short project objective statement
* dataset provenance and access notes
* model list and evaluation metrics
* example final results table
* citation information for the associated paper or preprint
* acknowledgment of collaborators/supervisors

---

## Citation

If you use this repository, cite the associated manuscript or project repository once the formal paper citation is available.

```bibtex
@misc{almalki_abr_ml,
  author       = {Zohoor Almalki},
  title        = {ABR_ML: Antibiotic Resistance Prediction},
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

For questions or collaboration related to this repository, please open an issue in the repository or contact the maintainer through the associated GitHub profile.
