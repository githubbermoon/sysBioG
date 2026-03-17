# Leakage Audit (Step 1 Evidence)

Date: March 17, 2026  
Audit notebook: `notebooks/01_data_audit.ipynb`

## Scope

This audit verifies that Step 1 has explicit controls for:
- target leakage
- identifier leakage
- split leakage
- progression/time proxy leakage

## Check Matrix

| Check | Rule | Status | Evidence |
|---|---|---|---|
| Target leakage | `CDR` used only to derive label; never a predictor | PASS | `docs/feature_leakage_plan.md`, notebook decision cell |
| Label-proxy leakage | `Group` excluded from main model | PASS | `docs/feature_leakage_plan.md` |
| Near-target proxy leakage | `MMSE` excluded from main model; optional later ablation only | PASS | `docs/feature_leakage_plan.md` |
| Identifier leakage | `Subject.ID`, `MRI.ID`, `rownames` excluded from predictors | PASS | `docs/feature_leakage_plan.md` |
| Split leakage | subject-level split only; no subject in both train and validation | PASS | notebook split assertion (`subject_overlap_count == 0`) |
| Temporal/progression proxy risk | `Visit`, `MR.Delay` not in locked main feature set for Step 2 baseline | PASS | this audit decision lock |

## Explicit Split Leakage Result

Deterministic subject split (`sha256(subject_id) % 10`, train if `< 8`, else validation):

| Metric | Value |
|---|---:|
| Train subjects | 117 |
| Validation subjects | 33 |
| Subject overlap | 0 |

Assertion outcome from notebook logic:
- `PASS: No subject appears in both train and validation splits.`

## Excluded Features (main model)

- `CDR` (label source)
- `Group` (label proxy)
- `MMSE` (high-risk near-target proxy)
- `Subject.ID` (identifier)
- `MRI.ID` (identifier)
- `rownames` (index artifact)
- `Hand` (low-value/noise feature for v1)

## Decisions Locked for Step 2

1. Use baseline-only cohort (one row per subject).
2. Use subject-level splits only.
3. Keep `MMSE` out of main model; permit only in a separate ablation experiment.
4. Keep `Visit` and `MR.Delay` out of baseline feature set to avoid progression leakage in v1.

