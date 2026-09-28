# Data Dictionary

Source: Synthea synthetic patient generator, CSV export (1,120 patients, seed 42, generated Sept 2026). All records are synthetic; no PHI.

## Source tables

| Table | Grain | Rows | Code system | Used for |
|---|---|---|---|---|
| patients | One row per patient | 1,120 | none | Cohort, demographics, age |
| encounters | One row per visit or stay | 62,323 | SNOMED CT (encounter type) | Utilization, cost, returns |
| conditions | One row per diagnosis episode | 39,176 | SNOMED CT | Condition prevalence, chronic flags |
| observations | One row per measurement | 805,560 | LOINC | Validation only |
| procedures | One row per procedure | 173,267 | SNOMED CT | Join-multiplication check |
| medications | One row per prescription | 52,434 | RxNorm | Validation only |

Identifier columns (SSN, driver's license, passport, names, address, ZIP, coordinates) are dropped from `patients` at load.

## Key fields

| Table.Field | Type | Meaning | Notes |
|---|---|---|---|
| patients.Id | text (UUID) | Patient key | Unique |
| patients.BIRTHDATE | date | Date of birth | |
| patients.DEATHDATE | date | Date of death | Null = living |
| patients.GENDER / RACE / ETHNICITY | text | Demographics | Synthea categories |
| encounters.Id | text (UUID) | Encounter key | Unique |
| encounters.PATIENT | text | → patients.Id | No orphans found |
| encounters.START / STOP | timestamp (UTC) | Visit start and end | No missing STOP |
| encounters.ENCOUNTERCLASS | text | ambulatory, wellness, outpatient, urgentcare, emergency, inpatient, home, snf, hospice, virtual | |
| encounters.DESCRIPTION | text | Encounter type | "Death Certification" is administrative; excluded |
| encounters.TOTAL_CLAIM_COST | numeric | Simulated total cost | Not real-world prices |
| encounters.PAYER_COVERAGE | numeric | Portion paid by payer | Never exceeds cost |
| conditions.PATIENT / ENCOUNTER | text | → patients / encounters | |
| conditions.START / STOP | date | Onset and resolution | Null STOP = still active |
| conditions.DESCRIPTION | text | SNOMED CT term | Suffix shows type: (disorder), (finding), (situation) |
| observations.ENCOUNTER | text | → encounters.Id | Null for yearly QALY/DALY/QOLS scores |

## Derived views

| View.Field | Definition |
|---|---|
| v_encounters | Encounters starting 2017-01-01 to 2025-12-31, Death Certification excluded |
| v_encounters.is_acute | 1 if encounter class is emergency or inpatient |
| v_encounters.duration_hours | (STOP − START) in hours |
| v_cohort | Patients born by 2025-12-31 and alive on or after 2017-01-01 |
| v_cohort.age | Age at 2025-12-31, or at death if earlier |
| v_cohort.age_band | 0-17, 18-44, 45-64, 65+ |

## Chronic-condition groups (notebook Part 3, Q10)

Matched on condition description, onset by 2025-12-31:

| Group | Match |
|---|---|
| Hypertension | contains "hypertension" |
| Diabetes | contains "diabetes", excluding "Prediabetes" |
| Chronic kidney disease | contains "chronic kidney disease" |
| COPD | contains "chronic obstructive" or "emphysema" |
| Asthma | contains "asthma" |
| Obesity | starts with "Body mass index 30+" |
| Chronic pain | starts with "Chronic pain" |
