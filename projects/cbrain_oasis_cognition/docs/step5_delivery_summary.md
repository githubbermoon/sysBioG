# Step 5: Final Write-up and Transfer Packaging

Date: March 23, 2026 (IST)

## What this step does

Step 5 finalizes the project packet for reproducible review and downstream handoff.
It does not retrain models; it consolidates frozen artifacts from Steps 1-4.

## Frozen Status

- Cohort and leakage contracts are locked from Step 2A.
- Preprocessing is train-fit-only and frozen from Step 2B.
- Baseline logistic and RF comparison metrics are fixed from Step 3A and 3B.
- Threshold, calibration, and error analyses are fixed from Step 3C and 3D.
- Interpretation guardrails are fixed from Step 4.

## Final Validation Snapshot

- Logistic ROC-AUC: 0.7782
- Logistic PR-AUC: 0.7983
- RF ROC-AUC: 0.7368
- RF PR-AUC: 0.6585
- Subject overlap (train vs validation): 0

## Transfer Artifacts

- results/modeling/step5_project_snapshot.json
- docs/project_brief.md
- docs/application_mapping.md
- docs/interpretation_step4.md

## Immediate Next Work

1. Replace single split with nested CV or repeated subject-level resampling.
2. Lock operating threshold on untouched holdout cohort.
3. Expand uncertainty reporting (bootstrap CIs for AUC, F1, calibration).
