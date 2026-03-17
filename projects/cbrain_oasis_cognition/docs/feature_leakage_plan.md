# Feature and Leakage Plan (Step 1)

## Candidate Predictors

| Field | Keep? | Rationale |
|---|---|---|
| `Age` | Yes | Core aging-related predictor |
| `Gender` | Yes | Demographic covariate |
| `EDUC` | Yes | Education may proxy cognitive reserve |
| `SES` | Yes (with missingness handling) | Useful socioeconomic context |
| `eTIV` | Yes | Structural normalization context |
| `nWBV` | Yes | Brain volume-related measure |
| `ASF` | Yes | Brain-structure scaling measure |
| `MR.Delay` | Conditional | May encode visit timing; validate for leakage risk |
| `Visit` | Conditional | Could be proxy for progression; include only if leakage-safe |

## Exclude from Predictors

| Field | Why exclude |
|---|---|
| `CDR` | Target source (`cognitive_impairment`) |
| `Group` | Likely diagnosis proxy, high leakage risk |
| `MMSE` | Highly proximate cognitive status indicator; can inflate performance |
| `Subject.ID` | Identifier only |
| `MRI.ID` | Identifier only |
| `rownames` | Index artifact |
| `Hand` | Low expected signal; optional, likely noise |

## Target Definition

- `cognitive_impairment = 1 if CDR > 0 else 0`

## Leakage Controls

1. Split by subject, not by row, to prevent same-person leakage across train/validation.
2. Prefer one baseline visit per subject for the first model version.
3. Build target before preprocessing and keep target fields out of feature matrix.
4. Freeze feature list before model comparison to avoid post-hoc leakage.

## Open Questions for Next Step

1. Should baseline be defined as minimum `Visit` or minimum `MR.Delay`?
2. Should `MMSE` be used only in an ablation experiment and excluded from the main result?
3. Is class imbalance high enough to require weighted loss and threshold tuning?

