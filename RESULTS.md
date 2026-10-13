# Saved Model Results

These values are copied from already-saved notebook outputs. The notebook was not executed again during this packaging process.

## Decision Tree Classifier

- **Accuracy:** 0.813925 (81.39%)
- **Test support:** 26,140 samples
- **Confusion matrix:**

```text
[[ 5249  2385]
 [ 2479 16027]]
```

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| 0 | 0.68 | 0.69 | 0.68 | 7,634 |
| 1 | 0.87 | 0.87 | 0.87 | 18,506 |
| Macro average | 0.77 | 0.78 | 0.78 | 26,140 |
| Weighted average | 0.81 | 0.81 | 0.81 | 26,140 |

## Tuned Random Forest Classifier

- **Accuracy:** 0.867048 (86.70%)
- **Test support:** 22,918 samples
- **Confusion matrix:**

```text
[[ 5045  1672]
 [ 1375 14826]]
```

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| 0 | 0.79 | 0.75 | 0.77 | 6,717 |
| 1 | 0.90 | 0.92 | 0.91 | 16,201 |
| Macro average | 0.84 | 0.83 | 0.84 | 22,918 |
| Weighted average | 0.87 | 0.87 | 0.87 | 22,918 |

## Saved best hyperparameters

```python
{
    "n_estimators": 300,
    "min_samples_split": 2,
    "min_samples_leaf": 2,
    "max_features": "sqrt",
    "max_depth": 40,
    "bootstrap": False,
}
```

## Important comparison warning

The Decision Tree report totals 26,140 examples, while the tuned Random Forest report totals 22,918 examples. The saved `df.info()` output shows 130,700 rows, which would normally produce a 26,140-row test split at `test_size=0.2`. Further, the search output log says 300 fits, while the displayed code specifies 5-fold CV and `n_iter=10`, which would normally request 50 fits. These are signs that notebook code and saved outputs may not all come from the same execution state.

Therefore, the reported scores are retained for transparency but **should not be presented as a fair model comparison until the notebook is rerun consistently after fixing the evaluation setup**.
