# Executive Summary: Synthetic Patient Utilization & Outcomes

**Data:** Synthea synthetic records · 1,034 patients · 39,902 encounters · 2017–2025
**Tools:** Python, SQLite, SQL, pandas, matplotlib
**Note:** All results describe a simulated population. They show associations, not causes, and are not estimates for any real population.

## Findings

1. **Age drives volume, and acute care even more.** Patients 65+ averaged 74 encounters over nine years versus 34 for ages 18–44. For emergency and inpatient care the gap widens: 4.5 acute encounters per patient versus 1.9.

2. **Acute care is concentrated in a small group.** The top 10% of acute users account for 52% of all ED and inpatient encounters. Across all encounter types, the top 10% of patients generate 38% of volume and 36% of cost.

3. **Inpatient stays carry the cost.** Inpatient stays are 1.3% of encounters but 11.6% of synthetic cost, with a median stay of 4 days.

4. **Returns after hospital stays are common.** 29% of inpatient episodes were followed by another ED visit or admission within 30 days, compared with 15% after an ED visit (18% across all acute episodes).

5. **Chronic kidney disease and diabetes stand out.** Among patients aged 45–64, those with chronic kidney disease had 3.2 times as many encounters as those without, and those with diabetes 2.7 times, with roughly double the acute encounters.

## What the data-quality work changed

- **Death Certification records** are filed in Synthea as wellness encounters up to 14 days after death (113 records). They were excluded so they would not count as care.
- **Analysis window** set to 2017–2025, where the record is complete; earlier years are sparse.
- **2021 volume spike** (+27%) traced to about 1,200 COVID-19 vaccination encounters, and treated as a one-off rather than a trend.
- **Join inflation** tested: joining encounters to procedures before aggregating overstates cost about sevenfold. All analyses aggregate first.

## Limitations

Synthetic data; simulated costs; chronic-condition comparisons partly reflect scheduled monitoring built into Synthea's care models; the 30-day return measure is operational and does not follow CMS readmission methodology.
