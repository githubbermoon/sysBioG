# Project Brief: Leakage-Safe Cognitive Impairment Prediction from OASIS Structured Cohort

Date: March 18, 2026 (IST)  
Project: `cbrain_oasis_cognition`

## 1) Problem

Predict cognitive impairment proxy status from structured brain-health cohort variables while enforcing strict leakage prevention and reproducibility.

Target:
- `cognitive_impairment = int(CDR > 0)`

Primary goal:
- scientific credibility (clean cohort logic, leakage control, transparent limits), not aggressive metric chasing.

## 2) Data and Cohort

Dataset:
- `data/oasis_dementia_mri_df.csv`
- 373 rows, 16 columns, 150 subjects

Cohort construction:
- Baseline-only, one row per `Subject.ID`
- Tie-breaks: `(Visit asc, MR.Delay asc, MRI.ID asc)`

Split:
- Deterministic subject-level split with `sha256(subject_id) % 10`
- Train if `<8`, else validation
- Train subjects: 117, validation subjects: 33, overlap: 0

## 3) Leakage Controls

Excluded from predictors:
- `CDR`, `Group`, `MMSE`, `Subject.ID`, `MRI.ID`, `rownames`, `Hand`, `Visit`, `MR.Delay`

Retained baseline predictors:
- `Age`, `Gender`, `EDUC`, `SES`, `eTIV`, `nWBV`, `ASF`

Preprocessing fit policy:
- Fit on train only
- Apply frozen params to validation
- SES median imputation + SES-missing indicator
- Train-defined gender encoding
- Train-fit z-score scaling for numeric predictors

## 4) Models and Evaluation

Baseline model:
- `sklearn` logistic regression (`liblinear`, `C=1.0`, `random_state=42`)

Comparison model:
- `sklearn` random forest (`n_estimators=500`, `min_samples_leaf=2`, `class_weight=balanced`, `random_state=42`)

Metrics tracked:
- ROC-AUC, PR-AUC, accuracy, precision, recall, F1, confusion matrix
- threshold sweep (Step 3C)
- calibration diagnostics (Brier, log-loss, ECE/MCE)

## 5) Results Summary (Validation)

Baseline logistic (`threshold=0.50`):
- ROC-AUC `0.7782`
- PR-AUC `0.7983`
- Accuracy `0.6970`
- Precision `0.6667`
- Recall `0.5714`
- F1 `0.6154`
- Confusion: TP=8, TN=15, FP=4, FN=6

Random forest comparison:
- ROC-AUC `0.7368` (worse than baseline)
- PR-AUC `0.6585` (worse than baseline)
- Accuracy `0.6970` (same)
- F1 `0.6429` (slightly higher)

Interpretation:
- Random forest did not improve ranking quality; logistic remains main model.

## 6) Threshold + Calibration + Errors

Threshold analysis (validation, exploratory):
- Best threshold by F1/Youden/balanced accuracy: `0.55`
- At 0.55: F1 `0.6957`, accuracy `0.7879`, specificity `0.9474`, recall `0.5714`

Calibration (validation):
- Brier `0.1806`
- log-loss `0.5366`
- ECE(10 bins) `0.1602`

Error analysis:
- Validation error rate `0.3030` (10/33)
- Error concentration near decision boundary bands (`[0.4,0.6)` highest error rate)
- Patterns reported as descriptive only (non-causal)

## 7) Scientific Limitations

- Small validation sample limits confidence in subgroup claims.
- Single deterministic split; no external cohort yet.
- Structured variables only (no high-dimensional imaging features in this phase).
- Calibration is moderate, not strong.
- Findings are predictive associations, not biological causality.

## 8) Why this aligns with CBRAIN

- Cohort-first thinking with explicit subject-level split and leakage gates.
- Reproducible, auditable pipeline from raw table to model diagnostics.
- Conservative interpretation and explicit limitations, aligned with biomedical research standards.
