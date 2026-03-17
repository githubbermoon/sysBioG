# Data Inspection Report (Step 1 Evidence)

Date: March 17, 2026  
Notebook evidence: `notebooks/01_data_audit.ipynb`  
Dataset: `data/oasis_dementia_mri_df.csv`

## Dataset Shape and Schema

| Item | Value |
|---|---|
| Rows | 373 |
| Columns | 16 |
| Unique subjects (`Subject.ID`) | 150 |
| Max visits per subject | 5 |

Columns:
- `rownames`, `Subject.ID`, `MRI.ID`, `Group`, `Visit`, `MR.Delay`, `Gender`, `Hand`, `Age`, `EDUC`, `SES`, `MMSE`, `CDR`, `eTIV`, `nWBV`, `ASF`

## Basic Type Profile (inferred)

| Column | Inferred type | Missing |
|---|---|---|
| `rownames` | int | 0 |
| `Subject.ID` | string | 0 |
| `MRI.ID` | string | 0 |
| `Group` | string | 0 |
| `Visit` | int | 0 |
| `MR.Delay` | int | 0 |
| `Gender` | string | 0 |
| `Hand` | string | 0 |
| `Age` | int | 0 |
| `EDUC` | int | 0 |
| `SES` | int | 19 |
| `MMSE` | int | 2 |
| `CDR` | float | 0 |
| `eTIV` | int | 0 |
| `nWBV` | float | 0 |
| `ASF` | float | 0 |

## Repeated-Visit Structure

| Metric | Value |
|---|---|
| Subjects with repeated visits (>1 row) | 150 |
| Visit-count distribution by subject | `{2: 94, 3: 43, 4: 9, 5: 4}` |
| Duplicate `(Subject.ID, Visit)` keys | 0 |
| Duplicate `MRI.ID` keys | 0 |
| Duplicate `rownames` keys | 0 |

## Target Prevalence

Target definition: `cognitive_impairment = 1 if CDR > 0 else 0`

| Cohort view | N | Positive | Negative | Positive rate |
|---|---:|---:|---:|---:|
| Full row-level table | 373 | 167 | 206 | 0.4477 |
| Baseline-only subject cohort | 150 | 65 | 85 | 0.4333 |

## Baseline Visit Rule (locked)

For each subject, select the earliest row by:
1. minimum `Visit`
2. tie-breaker minimum `MR.Delay`
3. tie-breaker lexical `MRI.ID`

This creates one row per subject for first-pass modeling (`N = 150`).

## Step 1 Decision Lock for Step 2

- Use baseline-only cohort for first model version.
- Keep target as `CDR > 0` binary.
- Handle `SES` missingness explicitly in preprocessing (imputation strategy to be defined in Step 2).

