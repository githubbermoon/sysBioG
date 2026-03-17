# Data Access Assessment (March 17, 2026)

## Objective

Identify a dataset path that is both:
1. Scientifically aligned with CBRAIN 2026, and
2. Immediately accessible for a 3-day project sprint.

## Access Status Summary

| Source | Current access pattern | Immediate coding suitability | Notes |
|---|---|---|---|
| OASIS official portal | Request-based workflow for datasets | Medium | Best official source, but approval/request timing can delay start |
| ADNI | Account + data application/approval flow | Low | Strong dataset, but not suitable for 72-hour initial build |
| NIAGADS | Open portal exists; qualified access required for some resources | Medium | Useful for future extension, not the fastest first milestone |
| OASIS-derived Rdatasets mirror | Direct CSV download (`HTTP 200`) | High | Fastest route to begin coding now with transparent provenance note |

## Decision

Use the OASIS-derived `Rdatasets` CSV for immediate project execution, while documenting that the canonical official source is OASIS.

## Practical Download Endpoint (Verified)

- URL:  
  `https://raw.githubusercontent.com/vincentarelbundock/Rdatasets/master/csv/NeuroDataSets/oasis_dementia_mri_df.csv`
- Local file:  
  `../data/oasis_dementia_mri_df.csv`
- Verification result: endpoint reachable and downloaded successfully on March 17, 2026.

## Source Links

- OASIS: https://sites.wustl.edu/oasisbrains/
- ADNI support/access: https://adni.loni.usc.edu/support/
- NIAGADS open portal: https://dss.niagads.org/open-access-data-portal/
- NIAGADS qualified-access FAQ: https://www.niagads.org/faqs/what-qualified-access

## Risk Note

The `Rdatasets` CSV is a practical working mirror for rapid development.  
Before submission, add a brief provenance note in project documentation acknowledging this is a derived distribution and not direct pull from the official OASIS request pipeline.

