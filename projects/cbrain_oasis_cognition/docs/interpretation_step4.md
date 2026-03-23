# Step 4: Model Interpretation and Scientific Framing

Date: March 23, 2026 (IST)

## Scope

This stage translates frozen Step 3 outputs into a conservative interpretation layer.
No additional training or resplitting is performed.

## Quantitative Feature Signals

Cross-model view (logistic coefficients + random-forest importance):

| predictor             |   coefficient |   abs_coefficient |   logistic_rank |   importance |   rf_rank | direction            |
|:----------------------|--------------:|------------------:|----------------:|-------------:|----------:|:---------------------|
| SES_missing_indicator |        1.2847 |            1.2847 |               1 |       0.0463 |         8 | positive_association |
| Gender                |        1.0211 |            1.0211 |               2 |       0.0667 |         6 | positive_association |
| EDUC                  |       -0.7238 |            0.7238 |               3 |       0.1501 |         5 | negative_association |
| nWBV                  |       -0.7138 |            0.7138 |               4 |       0.1959 |         1 | negative_association |
| Age                   |       -0.4280 |            0.4280 |               5 |       0.1666 |         2 | negative_association |
| eTIV                  |       -0.2383 |            0.2383 |               6 |       0.1585 |         3 | negative_association |
| SES                   |       -0.2237 |            0.2237 |               7 |       0.0606 |         7 | negative_association |
| ASF                   |        0.1757 |            0.1757 |               8 |       0.1553 |         4 | positive_association |

## Scientific Reading (Non-Causal)

- nWBV and Age remain high-signal features across models, consistent with expected aging/atrophy associations.
- EDUC and SES should be treated as cognitive-reserve and socioeconomic context variables, not mechanistic biomarkers.
- Gender and SES_missing_indicator can reflect cohort or data-collection effects as much as biology.
- eTIV and ASF are structural scaling features and should not be overinterpreted in isolation.

## Strict Guardrails

- Associations only: this is not causal inference.
- Validation size is small (n=33), so rank stability is limited.
- Single split only; no external cohort or nested CV yet.
- Interpretation reflects current feature engineering choices and may shift after stronger uncertainty analyses.

## Artifacts

- results/modeling/step4_feature_interpretation.csv
- results/modeling/step4_interpretation_summary.json
