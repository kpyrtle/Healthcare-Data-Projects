# Hospital Readmissions Analysis

## Overview
Analysis of 30-day hospital readmission rates using a SQL Server database 
covering 554,000+ patient encounters. The goal was to identify which patient 
populations are at highest risk for readmission and what clinical and 
demographic factors are most predictive.

## Why Readmissions Matter
Unplanned 30-day readmissions are a key quality metric in healthcare. 
Hospitals with higher-than-expected rates face financial penalties from CMS 
under the Hospital Readmissions Reduction Program (HRRP). This analysis 
explores where those risks are concentrated and what drives them.

## Analysis Questions
1. What are the readmission rates by department and insurance type? [Readmissions by Department and Insurance Type](./docs/reports/Readmissions_by_Dept_Insurance.csv)
2. How do readmitted patients differ demographically and clinically? [Clinical Comparisons for various Demographics](./docs/reports/Demographics_Clinical_Comparison.csv)
3. Does prior Emergency Department (ED) utilization predict readmission risk? [Prior ED use as a Predictor](./docs/reports/Prior_ED_Use_As_Predictor.csv)
4. Can a simple risk score identify high-risk patients at admission? [High Risk Patient Profiles](./docs/reports/High_Risk_Patient_Profile.csv)

## Key Findings
- Uninsured patients show readmission rates of 22-26%, roughly 7-8 percentage 
  points higher than privately insured patients across the same departments.
- Readmitted patients show meaningfully elevated creatinine (1.34 vs 1.23) 
  and longer average length of stay (6 vs 5 days).
- Each additional prior ED visit in the 6 months before admission correlates 
  with a higher readmission rate, from 16.94% (0 visits) to 24.00% (7 visits).
- A composite risk score combining 6 clinical flags shows readmission rates 
  rising from 14.34% (score 0) to 42.11% (score 5).

## Tools Used
- SQL Server (T-SQL)
- Microsoft Excel
- [Google Data Studio](https://datastudio.google.com/reporting/87912a2f-6ebd-45eb-9f4f-cf8f02d4b74a) (visualizations)

## Repository Structure
```
Readmissions/
├── Healthcare_Dataset_Raw.csv       # Original synthetic dataset (~554k rows, 44 columns)
├── Healthcare_Cleaned_Full.csv      # De-identified, cleaned version (42 columns)
├── data_quality_report.txt          # Inventory of 24 intentional data quality issues
├── sql/
│   └── Readmissions_Analysis.sql    # T-SQL queries for all 4 analysis questions
├── docs/
│   ├── data-dictionary.md           # Full schema for all 6 database tables
│   ├── analyst-guide.md             # Step-by-step guide: clean → identify → analyze → visualize
│   └── reports/
│       ├── Readmissions_by_Dept_Insurance.csv
│       ├── Demographics_Clinical_Comparison.csv
│       ├── High_Risk_Patient_Profile.csv
│       └── Prior_ED_Use_As_Predictor.csv
```

## Data
- **Healthcare_Dataset_Raw.csv** — 554k-row synthetic dataset with 44 columns covering demographics, 
  vitals, labs, charges, and outcomes. Includes intentional data quality issues for cleaning practice.
- **Healthcare_Cleaned_Full.csv** — De-identified per HIPAA Safe Harbor (SHA-256 research IDs, 
  3-digit ZIP, birth year only). Adds charge correction and readmission missingness flags.
- **data_quality_report.txt** — Documents 24 categories of introduced issues (missing labs/vitals, 
  duplicate rows, date logic errors, typos, charge sign errors) used to validate the cleaning pipeline.

## Docs & Reports

**SQL queries:**
- [sql/Readmissions_Analysis.sql](sql/Readmissions_Analysis.sql) — Four T-SQL queries that produced the analysis reports: readmission rates by department and insurance, demographic/clinical comparison of readmitted vs. non-readmitted patients, prior ED utilization as a predictor, and a CTE-based composite risk score. Inline findings comments included.

**Reference docs:**
- [docs/data-dictionary.md](docs/data-dictionary.md) — Schema definitions for all 6 SQL Server tables 
  (patients, encounters, vitals, lab_results, encounter_outcomes, ed_utilization), with column types, 
  value ranges, and missing data notes.
- [docs/analyst-guide.md](docs/analyst-guide.md) — Walkthrough of the full analysis workflow: data 
  cleaning, readmission identification logic, analytical directions, visualization suggestions, and a 
  column reference table.

**Analysis reports (docs/reports/):**
- **Readmissions_by_Dept_Insurance.csv** — Readmission rates for every department × insurance type 
  combination. Shows the uninsured penalty is consistent across all departments.
- **Demographics_Clinical_Comparison.csv** — Side-by-side comparison of readmitted vs. non-readmitted 
  patients across 11 demographic and clinical variables.
- **High_Risk_Patient_Profile.csv** — Readmission rates by composite risk score (0–5). Clear 
  dose-response from 14% (score 0) to 42% (score 5).
- **Prior_ED_Use_As_Predictor.csv** — Readmission rates by prior 6-month ED visits (0–9). Linear 
  trend from 17% (0 visits) to 24% (7 visits).