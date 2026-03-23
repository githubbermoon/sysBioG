# Model Comparison (Step 3B)

Date: 2026-03-17  
Notebook: `notebooks/05_model_comparison_step3b.ipynb`

## Scope

Compare Step 3A baseline logistic model against one stronger model (`RandomForestClassifier`) **without changing split or preprocessing contracts**.

## Leakage Controls

- Inputs only from frozen Step 2 artifacts (`results/preprocessing/*`, `results/cohort/split_subjects.json`).
- Feature order locked to manifest.
- No subject overlap (`overlap_count=0`).
- No leakage fields present in model matrix.

## Models Compared

- Baseline: `baseline_logistic_sklearn` (from Step 3A artifact).
- Stronger model: `RandomForestClassifier(n_estimators=500, min_samples_leaf=2, class_weight=balanced, random_state=42)`.

## Validation Metrics

| Metric | Baseline Logistic | Random Forest | Delta (RF - Baseline) |
|---|---:|---:|---:|
| ROC-AUC | 0.7782 | 0.7368 | -0.0414 |
| PR-AUC | 0.7983 | 0.6585 | -0.1398 |
| Accuracy | 0.6970 | 0.6970 | 0.0000 |
| F1 | 0.6154 | 0.6429 | 0.0275 |

Validation confusion matrix (Random Forest):
- TP=9, TN=14, FP=5, FN=5

## Step 3B Artifacts

- `results/modeling/step3b_random_forest_metrics.json`
- `results/modeling/step3b_random_forest_validation_predictions.csv`
- `results/modeling/step3b_random_forest_feature_importance.csv`
- `results/modeling/model_comparison.json`
