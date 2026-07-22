# Early Benzodiazepine Sedation, Delirium, and Ventilator-Free Days

Public analysis repository for a **causal mediation study** in critically ill adults:
does early benzodiazepine sedation (versus non-benzodiazepine continuous sedation)
reduce ventilator-free days, and how much of that effect is transmitted through
incident CAM-ICU-positive delirium? Built on MIMIC-IV v3.1.

This repository is the **public side of the analysis**. It documents what was done
(code, derived tables, and figures). It does **not** contain MIMIC-IV patient-level
data.

## Data access (required)

All primary data must be obtained from PhysioNet under a credentialed data-use agreement:

- **MIMIC-IV** (version 3.1): https://physionet.org/content/mimiciv/3.1/

Point the paths in `final_script.ipynb` (the central `CONFIG` / `REQUIRED` block) to
your PhysioNet download directory, then re-run the notebook.

## What this repository contains

| Path | Description |
|------|-------------|
| `final_script.ipynb` | End-to-end analysis notebook (target-trial protocol table, broad early-eligible cohort + 48 h landmark subset, STROBE flow, Table 1, MICE, stabilized IPTW plus inverse-probability-of-selection weights with Love plot, weighted g-computation for natural and interventional direct/indirect effects, total effect in the broad cohort, Rubin multiple-imputation bootstrap inference, component associations, competing-risk cause-specific hazards, 28-day mortality companion, combined results panel, and a full sensitivity battery) |
| `tables/` | Derived analytic tables (target-trial protocol, baseline, mediation, balance, sensitivity, component associations, cause-specific hazards, mortality) |
| `figures/` | Publication figures (PNG, PDF, TIFF): DAG, STROBE flow, descriptives, positivity/weights, Love plot, combined mediation+forest+E-value panel |
| `manuscript/jicm/` | TE-led manuscript package for Journal of Intensive Care Medicine (LaTeX draft, supplement, STROBE/TARGET checklists, cover letter; subscription track, no APC) |
| `LICENSE` | MIT License (code and derived non-identifiable outputs only) |
| `README.md` | This file |

## Study design (brief)

- **Target trial:** a pre-specified protocol table (Table 0) fixes eligibility, treatment strategies, assignment, time zero, landmark, follow-up, competing events, causal contrasts, the primary analysis, a sensitivity hierarchy, and the frozen primary estimand
- **Unit of analysis:** first adult ICU stay with early invasive mechanical ventilation and continuous sedation in the first 24 h (the broad early-eligible cohort, ascertained at time zero)
- **Exposure (A), 0-24 h:** any benzodiazepine infusion (midazolam or lorazepam) versus propofol and/or dexmedetomidine without benzodiazepine
- **Mediator (M), after 48 h landmark:** incident CAM-ICU-positive delirium among patients delirium-free and CAM-assessable at the landmark; the landmark is a mediation restriction only, sensitivity at 24/48/72 h
- **Outcome (Y):** ventilator-free days to day 28 (death within 28 days scores 0), with time to liberation and 28-day mortality as co-primary competing-risk endpoints
- **Confounders (C):** demographics, admission type, insurance, ICU unit type, admission era; early physiology (GCS, MAP, HR, lactate, creatinine, bilirubin, platelets), a measured SOFA-lite severity score, and early vasopressor/opioid co-sedation; comorbidity burden; and a benzodiazepine-indication flag (alcohol/sedative use disorder, withdrawal, or seizure) added specifically to limit confounding by indication
- **Confounding control:** MICE for missing baseline covariates (fit on the broad cohort); stabilized IPTW for early benzodiazepine use with all retained confounders in the propensity model; inverse-probability-of-selection weights for landmark inclusion (remaining in ICU and being CAM-assessable past 48 h) so the mediation targets the broad population; the full confounder set entered symmetrically in the mediator and outcome models to control mediator-outcome confounding; a Love plot of absolute SMD before vs after weighting
- **Estimand:** total effect (broad cohort) and natural plus interventional direct/indirect effects through delirium (weighted g-computation with A x M interaction); multiple-imputation inference by Rubin's rules over a per-imputation bootstrap that re-estimates the treatment and selection weights each draw; component associations (A->M, M->Y, A->Y); competing-risk cause-specific hazards for liberation and death and a 28-day mortality risk ratio; and E-values (point and CI-anchored) for both the total effect and the natural indirect effect

## Software

Analyses were implemented in Python (pandas, numpy, scikit-learn, statsmodels, matplotlib). Exact package versions are recorded in the notebook.

## Citation and contact

If you use this code or the analytic design, please cite the accompanying manuscript (when available) and MIMIC-IV:

- Johnson et al. MIMIC-IV. PhysioNet. https://doi.org/10.13026/kpb9-mt58

Corresponding author: January G. Msemakweli (jmsemak1@jh.edu)

## License and ethics

- **Code and derived non-identifiable outputs** are released under the [MIT License](LICENSE).
- **MIMIC-IV** is not included here and remains subject to the PhysioNet Credentialed Health Data Use Agreement.
