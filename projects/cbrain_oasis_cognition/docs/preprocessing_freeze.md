# Preprocessing Freeze (Step 2B)

Date: 2026-03-17  
Notebook: `notebooks/03_preprocessing_freeze.ipynb`  
Inputs: `results/cohort/baseline_cohort_full.csv`, `results/cohort/split_subjects.json`

## Leakage-Safe Fit Protocol

All preprocessing parameters are fit on **train subjects only** and then applied unchanged to validation.

- Split rule source: `results/cohort/split_subjects.json`
- Subject overlap check: **0**
- Leakage fields in model matrix: **not present**

## Schema and Transform Rules

Raw candidate predictors:
- `Age`, `Gender`, `EDUC`, `SES`, `eTIV`, `nWBV`, `ASF`

Locked exclusions:
- `CDR`, `Group`, `MMSE`, `Subject.ID`, `MRI.ID`, `rownames`, `Hand`, `Visit`, `MR.Delay`

Applied transforms:
- `SES`: median imputation from train only (median=2.0)
- `SES_missing`: binary missingness indicator
- `Gender`: ordinal encoding using train categories ['F', 'M'] with mapping {'F': 0, 'M': 1}
- numeric standardization (z-score) fit on train only for `Age`, `EDUC`, `SES_imputed`, `eTIV`, `nWBV`, `ASF`

Final feature order:
- `Age_z`, `Gender_code`, `EDUC_z`, `SES_imputed_z`, `SES_missing`, `eTIV_z`, `nWBV_z`, `ASF_z`

## Train/Validation Label Balance

| Split | Rows | Positive | Negative | Positive Rate |
|---|---:|---:|---:|---:|
| Train | 117 | 51 | 66 | 0.4359 |
| Validation | 33 | 14 | 19 | 0.4242 |

## Frozen Artifacts

- `results/preprocessing/X_train.csv`
- `results/preprocessing/X_validation.csv`
- `results/preprocessing/y_train.csv`
- `results/preprocessing/y_validation.csv`
- `results/preprocessing/feature_manifest.json`
- `results/preprocessing/preprocessing_fit.json`
- `results/preprocessing/preprocessing_summary.json`
