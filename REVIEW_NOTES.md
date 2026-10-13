# Review Notes — Before Publishing or Using the Scores

I inspected notebook source and saved outputs without executing the notebook. These are review findings, not claims about what a new run will produce.

## 1. Saved outputs are inconsistent

- `df.info()` reports 130,700 rows, 14 columns, and 130,699 non-null values in `addicted_label`.
- The saved Decision Tree classification report totals 26,140 test rows (consistent with 20% of 130,700).
- The saved tuned Random Forest report totals 22,918 test rows.
- The output from the search claims 300 fits, although the visible code specifies `cv=5` and `n_iter=10`, which ordinarily implies 50 candidate-fold fits.
- Some saved cells have missing execution counts or outputs, and the notebook appears to have outputs from different execution states.

**Recommendation:** clear stale outputs and rerun the notebook from top to bottom after fixing the issues below. Do not claim that 81.39% and 86.70% are a clean apples-to-apples comparison yet.

## 2. Preprocessing occurs before the split

Mean/median/mode imputation and StandardScaler transformations are fitted before `train_test_split`. This lets information from the future test set influence preprocessing. Put imputation, encoding, and scaling inside a scikit-learn `Pipeline`/`ColumnTransformer` fitted only on the training data.

## 3. RandomizedSearchCV uses the full dataset

The notebook uses `serch_cv.fit(x, y)` after making `x_train`, `x_test`, `y_train`, and `y_test`. That means the held-out test rows are used in cross-validation for hyperparameter selection. Instead, fit the search on `x_train, y_train` only, then evaluate the selected estimator once on `x_test, y_test`.

## 4. Missing target values need an explicit policy

The saved `df.info()` output indicates one missing value in `addicted_label`, while the later saved null-count output reports no missing target values. Since target labels should not be imputed like features, check the actual file and usually remove rows with missing targets before splitting.

## 5. Categorical encoding

- `stress_level` Low/Medium/High is ordinal, so ordered encoding may be appropriate if the competition definition supports it.
- `gender` is nominal, but the notebook maps Male/Female/Other to 1/0/2. This can introduce an artificial numerical order; one-hot encoding is usually safer for a nominal feature.
- Put encoding in the preprocessing pipeline to ensure consistent training/test transformations.

## 6. Reproducibility

The final `RandomForestClassifier` cell doesn't set `random_state`, so repeated fits may differ. Set a fixed random seed when reproducibility is desired. Save the final pipeline and feature schema only after validating it correctly.

## 7. Source and licensing

The notebook uses `/content/train.csv`; it does not provide the exact Kaggle competition URL or dataset license. Add the correct competition link and check Kaggle's data/competition terms before redistributing any dataset. This package intentionally does not include `train.csv`.

## 8. Scope / responsible presentation

Describe this as an educational Kaggle binary-classification experiment using a label named `addicted_label`. Do not call it a medical or psychological diagnostic tool; the notebook does not demonstrate clinical validity.
