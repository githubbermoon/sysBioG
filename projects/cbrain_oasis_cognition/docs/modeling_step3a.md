# Baseline Modeling (Step 3A)

Date: 2026-03-17  
Notebook: `notebooks/04_model_baseline_step3a.ipynb`

## Scope

This step trains a deterministic **scikit-learn logistic regression** baseline on frozen Step 2 preprocessing outputs.

## Leakage Controls

- Inputs come only from `results/preprocessing/` and subject split from `results/cohort/split_subjects.json`.
- Excluded leakage fields (`CDR`, `Group`, `MMSE`, identifiers, `Visit`, `MR.Delay`) are absent from model matrix.
- Subject overlap check remains **0**.
- No refit of preprocessing on validation.

## Model

- Family: `sklearn.linear_model.LogisticRegression`
- Hyperparameters: C=1.0, solver=liblinear, max_iter=2000, random_state=42 (default L2).
- Feature order: `Age_z`, `Gender_code`, `EDUC_z`, `SES_imputed_z`, `SES_missing`, `eTIV_z`, `nWBV_z`, `ASF_z`.

## Metrics (Threshold = 0.5)

| Split | ROC-AUC | PR-AUC | Accuracy | Precision | Recall | F1 |
|---|---:|---:|---:|---:|---:|---:|
| Train | 0.8004 | 0.7995 | 0.7436 | 0.7561 | 0.6078 | 0.6739 |
| Validation | 0.7782 | 0.7983 | 0.6970 | 0.6667 | 0.5714 | 0.6154 |

Validation confusion matrix counts:
- TP=8, TN=15, FP=4, FN=6

## Artifacts

- `results/modeling/baseline_logistic_coefficients.csv`
- `results/modeling/baseline_metrics.json`
- `results/modeling/baseline_train_predictions.csv`
- `results/modeling/baseline_validation_predictions.csv`
- `results/modeling/baseline_model_card.json`
