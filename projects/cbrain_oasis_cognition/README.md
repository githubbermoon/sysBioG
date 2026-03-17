# CBRAIN OASIS Cognition (Step 1)

## Problem Statement

Build a leakage-safe machine learning workflow to predict cognitive impairment proxy status from structured brain-health cohort data.  
The core objective is to show rigorous data curation, defensible feature selection, and honest evaluation, not leaderboard-style optimization.

## Why This Matters

Brain-aging cohorts are noisy, partially missing, and prone to leakage mistakes.  
A clean, reproducible workflow on structured cohort data is directly aligned with CBRAIN 2026's focus on data curation and analysis for cognitive impairment.

## Dataset Choice

- Primary working dataset (immediately accessible):  
  `oasis_dementia_mri_df.csv` from the `Rdatasets` mirror of OASIS structured data.
- Local path:  
  `data/oasis_dementia_mri_df.csv`

## Target Variable

- Binary target: `cognitive_impairment = 1 if CDR > 0 else 0`
- `CDR` is used only to create the target and then excluded from predictors.

## Success Criteria

1. Implement leakage-safe cohort split logic.
2. Build interpretable baseline and one stronger comparison model.
3. Report ROC-AUC, PR-AUC, confusion matrix, and key limitations.
4. Provide biological interpretation without causal overclaiming.

## Step 1 Outputs Completed

- Project question and target defined.
- Dataset access routes assessed (official and practical).
- Initial feature and leakage plan created in `docs/feature_leakage_plan.md`.
- Data access assessment captured in `docs/data_access_assessment.md`.
- Reproducible inspection notebook added: `notebooks/01_data_audit.ipynb`.
- Executed inspection evidence captured in `docs/data_inspection_report.md`.
- Executed leakage checks captured in `docs/leakage_audit.md`.
