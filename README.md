# Smartphone Addiction Prediction

A Kaggle competition machine-learning project exploring whether a record is labeled as smartphone-addicted using usage patterns and other provided features. This repository documents the work saved in the original notebook; model code has not been rerun as part of preparing this repository.

> **Scope note:** This is an educational classification exercise on a dataset label. It is not a clinical assessment, validated psychological instrument, or tool for diagnosing smartphone addiction in people.

## Project Overview

The notebook loads `train.csv`, explores missing values and numeric distributions, preprocesses the available columns, and compares classification approaches. It includes a Decision Tree baseline and a Random Forest workflow with `RandomizedSearchCV` hyperparameter search.

## Dataset

- **Source:** Kaggle competition (add the exact competition URL here; the URL is not recorded in the notebook).
- **Input file expected by notebook:** `train.csv`.
- **Target column:** `addicted_label`.
- **Problem type:** Binary classification (`0` / `1`).
- **Dataset file:** Not included in this repository. Download it from the original Kaggle source and follow its terms and competition rules.

The saved `df.info()` output shows 130,700 rows and 14 columns, but other saved outputs in the notebook imply different sample counts. See [Review Notes](REVIEW_NOTES.md) before interpreting the reported metrics.

## Features Used

The notebook uses these predictors after dropping `id` and `addicted_label`:

- Age
- Daily screen time
- Social media hours
- Gaming hours
- Work/study hours
- Sleep hours
- Notifications per day
- App opens per day
- Weekend screen time
- Gender
- Stress level
- Academic work impact

Column names above are taken from the notebook. Their meanings and measurement validity depend on the Kaggle dataset documentation.

## Workflow in the Notebook

1. Load the training CSV and inspect its columns and missing values.
2. Plot histograms and box plots for numerical columns with missing values. Saved plots are included in [`results/eda/`](results/eda/).
3. Impute `age` with the mean; impute the other selected numerical columns with their medians.
4. Impute missing categorical values with the mode.
5. Encode `academic_work_impact` as `0`/`1`, `stress_level` as `0`/`1`/`2`, and `gender` as integer categories.
6. Standardize the selected numerical features.
7. Split data with `train_test_split(test_size=0.2, random_state=42)`.
8. Train a Decision Tree classifier.
9. Set up a Random Forest classifier and tune hyperparameters with `RandomizedSearchCV`.
10. Evaluate the final tuned Random Forest using accuracy, a confusion matrix, and a classification report.

## Saved Results

The notebook contains these saved evaluation outputs:

| Model / saved output | Accuracy | Notes |
|---|---:|---|
| Decision Tree | 81.39% | Saved report uses 26,140 test samples. |
| Tuned Random Forest | 86.70% | Saved report uses 22,918 test samples; output-count mismatch means this is not directly comparable with the Decision Tree score. |

The saved tuned Random Forest parameters are:

- `n_estimators`: 300
- `max_depth`: 40
- `min_samples_split`: 2
- `min_samples_leaf`: 2
- `max_features`: `sqrt`
- `bootstrap`: `False`

Detailed saved metrics and confusion matrices are in [RESULTS.md](RESULTS.md). These numbers are copied from notebook outputs, not freshly verified by rerunning the experiment.

## How to Open the Notebook

1. Install Python.
2. Install dependencies with `pip install -r requirements.txt`.
3. Download `train.csv` from the original Kaggle competition and place it where the notebook expects it, or update the CSV path.
4. Open `mobile-phone.ipynb` in Jupyter Notebook, JupyterLab, or VS Code.
5. Read [REVIEW_NOTES.md](REVIEW_NOTES.md) before rerunning or using the saved scores. The notebook code has unresolved evaluation and saved-output consistency issues.

## Repository Structure

```text
smartphone-addiction-prediction/
├── mobile-phone.ipynb
├── README.md
├── RESULTS.md
├── REVIEW_NOTES.md
├── requirements.txt
├── .gitignore
└── results/
    └── eda/
        └── (saved histogram and box-plot images)
```

## Limitations

- Dataset provenance, license, and exact competition URL must be documented from the original Kaggle page.
- Saved notebook outputs are inconsistent in sample counts and search logs.
- Preprocessing is performed before the train/test split, so test information can influence imputation and scaling.
- Hyperparameter search is fit on `x, y` (the full data) rather than only `x_train, y_train`, which compromises the held-out test evaluation.
- Integer coding for `gender` may imply an order that the categories do not have.
- No clinical validity is established by this competition model.

## Acknowledgements

Dataset and competition: [https://www.kaggle.com/competitions/playground-series-s6e8].
