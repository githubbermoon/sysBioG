# Threshold and Calibration Analysis (Step 3C)

Date: 2026-03-17  
Notebook: `notebooks/06_threshold_calibration_step3c.ipynb`

## Scope

Analyze threshold trade-offs and calibration for the frozen Step 3A baseline probabilities.

## Threshold Sweep (Validation)

- Checked thresholds from 0.05 to 0.95 (step 0.01).
- At threshold 0.50: F1=0.6154, Recall=0.5714, Specificity=0.7895.
- Best by F1: threshold=0.55, F1=0.6957.
- Best by Youden's J: threshold=0.55, J=0.5188.
- Best by Balanced Accuracy: threshold=0.55, BA=0.7594.

## Calibration Summary

- Validation Brier score: 0.1806
- Validation log loss: 0.5366
- Validation ECE (10 bins): 0.1602
- Validation MCE (10 bins): 0.2946

Train-reference (overfit context):
- Train Brier score: 0.1772
- Train ECE (10 bins): 0.1059

## Interpretation Guardrail

Threshold optimization performed on validation should be treated as exploratory.
For a production or claim-level threshold, lock it via a separate holdout or nested CV protocol.

## Step 3C Artifacts

- `results/modeling/step3c_threshold_sweep.csv`
- `results/modeling/step3c_threshold_analysis.json`
- `results/modeling/step3c_calibration_bins.csv`
- `results/modeling/step3c_calibration_summary.json`
