# Cohort Construction (Step 2A)

Date: 2026-03-17  
Notebook: `notebooks/02_cohort_construction.ipynb`  
Source data: `data/oasis_dementia_mri_df.csv`

## Cohort Rule and Tie-Break Policy

Baseline cohort includes exactly one row per `Subject.ID`, selected with key priority:
1. minimum `Visit`
2. tie-break minimum `MR.Delay`
3. tie-break lexical `MRI.ID`

Resulting baseline size: **150 subjects** (from 373 raw rows, 150 unique subjects).

## Locked Feature Policy

Excluded columns for main baseline model:
- `CDR`, `Group`, `MMSE`, `Subject.ID`, `MRI.ID`, `rownames`, `Hand`, `Visit`, `MR.Delay`

Retained baseline v1 predictors:
- `Age`, `Gender`, `EDUC`, `SES`, `eTIV`, `nWBV`, `ASF`

Model label:
- `cognitive_impairment = int(CDR > 0)`

## Split Method and Leakage Assertion

Deterministic split at **subject level only**:
- bucket = `sha256(subject_id) % 10`
- train if bucket `< 8`, validation otherwise

Leakage assertions:
- subject overlap count: **0**
- excluded columns absent from `baseline_model_table.csv`: **PASS**
- `cognitive_impairment` domain and missingness checks: **PASS**

## Train/Validation Class Balance

| Split | Rows | Positive | Negative | Positive Rate |
|---|---:|---:|---:|---:|
| Train | 117 | 51 | 66 | 0.4359 |
| Validation | 33 | 14 | 19 | 0.4242 |

## Frozen Artifacts

- `results/cohort/baseline_cohort_full.csv`
- `results/cohort/baseline_model_table.csv`
- `results/cohort/split_subjects.json`
- `results/cohort/cohort_summary.json`
