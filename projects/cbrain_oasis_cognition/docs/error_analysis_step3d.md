# Error Analysis (Step 3D)

Date: 2026-03-18  
Notebook: `notebooks/07_error_analysis_step3d.ipynb`

## Scope

Descriptive error analysis on validation predictions from Step 3A baseline model (threshold=0.5).

## Overall Validation Error Profile

- N=33
- TP=8, TN=15, FP=4, FN=6
- Error count=10, Error rate=0.3030

## Group Patterns

Gender error rates:
- F: 0.222 (n=18), M: 0.400 (n=15)

SES missingness error rates:
- SES_missing=0: 0.303 (n=33)

Age-bin error rates:
- <70: 0.111 (n=9), 70-79: 0.438 (n=16), 80+: 0.250 (n=8)

Probability-band error rates:
- [0.0,0.2): 0.000 (n=4), [0.2,0.4): 0.385 (n=13), [0.4,0.6): 0.500 (n=8), [0.6,0.8): 0.143 (n=7), [0.8,1.0]: 0.000 (n=1)

## Boundary-Adjacent Errors (Closest to 0.5)

Top FPs near decision boundary:
[
  {
    "Subject.ID": "OAS2_0085",
    "y_probability": 0.5179382242638206,
    "Age": 78,
    "Gender": "F",
    "SES_missing": 0
  },
  {
    "Subject.ID": "OAS2_0126",
    "y_probability": 0.5340761357627312,
    "Age": 74,
    "Gender": "F",
    "SES_missing": 0
  },
  {
    "Subject.ID": "OAS2_0176",
    "y_probability": 0.5457624701377319,
    "Age": 84,
    "Gender": "M",
    "SES_missing": 0
  },
  {
    "Subject.ID": "OAS2_0091",
    "y_probability": 0.6126808561056267,
    "Age": 75,
    "Gender": "M",
    "SES_missing": 0
  }
]

Top FNs near decision boundary:
[
  {
    "Subject.ID": "OAS2_0060",
    "y_probability": 0.4764042465486557,
    "Age": 75,
    "Gender": "M",
    "SES_missing": 0
  },
  {
    "Subject.ID": "OAS2_0066",
    "y_probability": 0.37232774193823387,
    "Age": 61,
    "Gender": "M",
    "SES_missing": 0
  },
  {
    "Subject.ID": "OAS2_0146",
    "y_probability": 0.3465060626051767,
    "Age": 80,
    "Gender": "F",
    "SES_missing": 0
  },
  {
    "Subject.ID": "OAS2_0184",
    "y_probability": 0.33284743769228986,
    "Age": 72,
    "Gender": "F",
    "SES_missing": 0
  },
  {
    "Subject.ID": "OAS2_0179",
    "y_probability": 0.32648955559797554,
    "Age": 79,
    "Gender": "M",
    "SES_missing": 0
  }
]

## Interpretation Guardrail

These are descriptive cohort/model diagnostics only.
They indicate where the model struggles, but do not establish causal relationships.

## Step 3D Artifacts

- `results/modeling/step3d_error_analysis.csv`
- `results/modeling/step3d_error_group_summary.json`
