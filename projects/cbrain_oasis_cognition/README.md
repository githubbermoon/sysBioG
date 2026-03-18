# CBRAIN OASIS Cognition

Leakage-safe machine learning workflow to predict cognitive impairment proxy status from structured OASIS cohort data.

## Objective

Build a scientifically credible baseline pipeline for cognitive-impairment risk classification where data curation, leakage control, and interpretability are prioritized over leaderboard-style optimization.

## Dataset

- Source table: `data/oasis_dementia_mri_df.csv`
- Raw shape: `373 rows x 16 columns`
- Unique subjects: `150`
- Repeated-visit structure: `{2: 94, 3: 43, 4: 9, 5: 4}`

Target definition:
- `cognitive_impairment = int(CDR > 0)`

## Locked Method Decisions

- Baseline cohort: one row per `Subject.ID` using `(Visit asc, MR.Delay asc, MRI.ID asc)`
- Leakage exclusions from predictors:
  - `CDR`, `Group`, `MMSE`, `Subject.ID`, `MRI.ID`, `rownames`, `Hand`, `Visit`, `MR.Delay`
- Baseline v1 predictors:
  - `Age`, `Gender`, `EDUC`, `SES`, `eTIV`, `nWBV`, `ASF`
- Split policy:
  - subject-level only
  - deterministic: `sha256(subject_id) % 10` (`<8` train, else validation)
  - no subject overlap allowed

## Pipeline Status (Complete up to Step 3D)

1. **Step 1: data audit + leakage audit**
   - Notebook: `notebooks/01_data_audit.ipynb`
   - Docs: `docs/data_inspection_report.md`, `docs/leakage_audit.md`

2. **Step 2A: cohort construction freeze**
   - Notebook: `notebooks/02_cohort_construction.ipynb`
   - Outputs: `results/cohort/*`
   - Doc: `docs/cohort_construction.md`

3. **Step 2B: preprocessing freeze**
   - Notebook: `notebooks/03_preprocessing_freeze.ipynb`
   - Outputs: `results/preprocessing/*`
   - Doc: `docs/preprocessing_freeze.md`

4. **Step 3A: baseline model (scikit-learn logistic regression)**
   - Notebook: `notebooks/04_model_baseline_step3a.ipynb`
   - Outputs: `results/modeling/baseline_*`
   - Doc: `docs/modeling_step3a.md`

5. **Step 3B: stronger comparison model (random forest)**
   - Notebook: `notebooks/05_model_comparison_step3b.ipynb`
   - Outputs: `results/modeling/model_comparison.json`, `results/modeling/step3b_*`
   - Doc: `docs/model_comparison_step3b.md`

6. **Step 3C: threshold + calibration analysis**
   - Notebook: `notebooks/06_threshold_calibration_step3c.ipynb`
   - Outputs: `results/modeling/step3c_*`
   - Doc: `docs/threshold_calibration_step3c.md`

7. **Step 3D: error analysis**
   - Notebook: `notebooks/07_error_analysis_step3d.ipynb`
   - Outputs: `results/modeling/step3d_*`
   - Doc: `docs/error_analysis_step3d.md`

## Main Results (Validation)

### Baseline logistic (`threshold=0.50`)

- ROC-AUC: `0.7782`
- PR-AUC: `0.7983`
- Accuracy: `0.6970`
- Precision: `0.6667`
- Recall: `0.5714`
- F1: `0.6154`
- Confusion: `TP=8, TN=15, FP=4, FN=6`

### Comparison model (Random Forest)

- ROC-AUC: `0.7368` (delta vs baseline: `-0.0414`)
- PR-AUC: `0.6585` (delta vs baseline: `-0.1398`)
- Accuracy: `0.6970` (delta: `0.0000`)
- F1: `0.6429` (delta: `+0.0275`)

Interpretation:
- Random forest did not improve ranking quality (ROC-AUC/PR-AUC), so logistic remains the primary model.

### Step 3C highlights

- Best exploratory threshold on validation by F1/Youden/balanced accuracy: `0.55`
- At `0.55`: Accuracy `0.7879`, F1 `0.6957`, Specificity `0.9474`, Recall `0.5714`
- Calibration (validation):
  - Brier `0.1806`
  - ECE (10 bins) `0.1602`

Guardrail:
- Threshold optimization on validation is exploratory only; final threshold should be locked via external holdout or nested CV.

### Step 3D highlights

- Validation error rate: `10/33 = 0.3030`
- Error concentration is highest near boundary probability bands (`[0.4,0.6)`).
- Descriptive subgroup signal observed (e.g., by age/gender), but no causal interpretation claimed.

## Scientific Limitations

- Small validation sample (`n=33`) implies high uncertainty in subgroup and threshold conclusions.
- Single deterministic split used; no nested CV or external test cohort yet.
- Structured variables only; no imaging-derived high-dimensional features in this phase.
- Calibration is moderate; confidence interpretation should be cautious.
- All findings are associative/descriptive, not causal.

## Reproducibility

Set up environment:

```bash
cd /Users/pranjal/Projects/sysBio/projects/cbrain_oasis_cognition
python3 -m venv .venv
.venv/bin/python -m pip install --upgrade pip
.venv/bin/python -m pip install scikit-learn
```

Execute notebooks in order (recommended):

```bash
# Step 1
.venv/bin/python - <<'PY'
import json
from pathlib import Path
nb=Path('notebooks/01_data_audit.ipynb')
ns={}
for c in json.loads(nb.read_text())['cells']:
    if c.get('cell_type')=='code':
        exec(compile(''.join(c['source']), nb.name, 'exec'), ns, ns)
PY

# Step 2A-3D
for nb in \
  notebooks/02_cohort_construction.ipynb \
  notebooks/03_preprocessing_freeze.ipynb \
  notebooks/04_model_baseline_step3a.ipynb \
  notebooks/05_model_comparison_step3b.ipynb \
  notebooks/06_threshold_calibration_step3c.ipynb \
  notebooks/07_error_analysis_step3d.ipynb; do
  .venv/bin/python - <<PY
import json
from pathlib import Path
nb=Path('$nb')
ns={}
for c in json.loads(nb.read_text())['cells']:
    if c.get('cell_type')=='code':
        exec(compile(''.join(c['source']), nb.name, 'exec'), ns, ns)
PY
done
```

## CBRAIN Alignment

- Cohort-centric and leakage-aware workflow mirrors CBRAIN’s emphasis on rigorous biomedical data handling.
- Transparent exclusions and reproducible audit trail support scientific credibility.
- Interpretation and limitations are explicit, avoiding overclaiming from small-sample predictive modeling.
