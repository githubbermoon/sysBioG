# Application Mapping: Interview and Application Scripts

Date: March 18, 2026 (IST)

## 1) 90-Second Version

I built a leakage-safe machine-learning project on structured OASIS cohort data to predict cognitive-impairment proxy status. The project focus was scientific rigor, not just metric maximization.

I started by auditing the raw table, checking schema, missingness, repeated visits, and target prevalence. Then I locked a baseline-only cohort rule: one row per subject from the earliest visit with deterministic tie-breaks. I also locked high-risk leakage exclusions like CDR, Group, MMSE, IDs, and temporal fields.

I used a deterministic subject-level split with zero overlap and froze preprocessing using train-only fit statistics. The baseline logistic model reached about 0.78 ROC-AUC and 0.80 PR-AUC on validation. I compared a random forest under exactly the same frozen split and features; it did not improve ranking metrics, so I kept logistic as the primary model.

After that, I did threshold and calibration analysis plus error analysis. The key message is that the pipeline is reproducible and leakage-safe, with explicit limitations and no causal overclaims. This aligns with CBRAIN because it demonstrates cohort-oriented data discipline and transparent biomedical ML reasoning.

## 2) 3-Minute Version

I designed this project to demonstrate research-style ML discipline on cohort data, which is why I chose structured OASIS and focused heavily on leakage prevention.

First, I audited the dataset end-to-end: 373 rows, 16 columns, 150 subjects, repeated visits for all subjects, and controlled missingness primarily in SES and MMSE. I defined the target as `cognitive_impairment = int(CDR > 0)` and explicitly treated CDR as label-source only.

Second, I froze cohort and split policy before modeling. I built a baseline cohort with one row per subject using a deterministic rule: minimum Visit, then minimum MR.Delay, then MRI.ID tie-break. For splitting, I used subject-level deterministic hashing and enforced zero train/validation subject overlap.

Third, I froze preprocessing with train-only fit. I retained only leakage-safe baseline predictors: age, gender, education, SES, eTIV, nWBV, ASF. I used SES median imputation from train, added SES-missing indicator, encoded gender from train categories, and applied train-fit scaling.

Fourth, I trained and evaluated models. The baseline was scikit-learn logistic regression. Validation performance was roughly ROC-AUC 0.78 and PR-AUC 0.80. Then I ran a random-forest comparison model under the exact same frozen contracts. It had slightly better F1 but worse ROC-AUC and PR-AUC, so logistic remained the primary model due to better ranking quality and interpretability.

Fifth, I added threshold, calibration, and error analysis. Exploratory threshold sweep suggested 0.55 improves F1/accuracy on this validation split, but I explicitly flagged that threshold locking on validation is not final and needs external holdout or nested CV. Calibration was moderate, and error analysis showed mistakes are concentrated near decision-boundary probabilities.

The main outcome is a transparent, reproducible, leakage-safe pipeline from raw cohort data to model diagnostics, with explicit scientific limitations and non-causal interpretation. That is the strongest CBRAIN-relevant signal I wanted to show.

## 3) Application Form Mapping (Use Directly)

### Project Title
Leakage-Safe Cognitive Impairment Prediction from OASIS Structured Cohort Data

### Motivation
I wanted to show that I can handle biomedical cohort ML with strict data hygiene and honest evaluation, which is central to CBRAIN-style research.

### Methods (Short)
Baseline-only cohort per subject, subject-level deterministic split, leakage-safe feature set, train-only preprocessing fit, logistic baseline plus controlled random-forest comparison.

### Results (Short)
Logistic validation ROC-AUC ~0.78, PR-AUC ~0.80; random forest did not improve ranking metrics. Additional threshold/calibration/error analyses were performed with clear limitations.

### Limitations (Short)
Small validation sample, single split, moderate calibration, descriptive subgroup patterns only, no causal claims.

### Why this aligns with CBRAIN
The project emphasizes cohort construction, leakage prevention, reproducibility, and biologically careful interpretation over superficial model complexity.

## 4) Follow-up Questions You Should Expect

1. Why did you exclude MMSE and Group?
- They are high-risk target proxies; including them inflates apparent performance and weakens scientific credibility.

2. Why baseline-only cohort instead of longitudinal modeling?
- This first version isolates a clean leakage-safe baseline. Longitudinal modeling can be a next-phase extension.

3. Why keep logistic as primary if random forest had slightly higher F1?
- Random forest had worse ROC-AUC and PR-AUC on validation, so ranking quality was weaker. Logistic is also more interpretable.

4. Is threshold 0.55 your final operating point?
- Not yet. That threshold was chosen on validation and is exploratory; final operating threshold should be locked with separate holdout/nested CV.

5. Can this model be used clinically?
- Not in current form. This is a research prototype with clear limitations and no clinical deployment claims.
