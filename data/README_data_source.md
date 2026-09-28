# Data Source

Synthetic patient records generated with [Synthea](https://github.com/synthetichealth/synthea) (MITRE, Apache 2.0). The CSVs are not stored in this repository because the observations file exceeds GitHub's size limit.

Generation command (Java 17+):

```
java -jar synthea-with-dependencies.jar -p 1000 -s 42 --exporter.csv.export=true --exporter.fhir.export=false --exporter.hospital.fhir.export=false --exporter.practitioner.fhir.export=false --exporter.years_of_history=10 Illinois
```

Copy `patients.csv`, `encounters.csv`, `conditions.csv`, `observations.csv`, `procedures.csv` and `medications.csv` from `output/csv/` into `data/raw/`.

`-p 1000` sets the number of living patients; deceased patients are added on top (1,120 total). Synthea's latest build changes over time, so a regenerated dataset may differ slightly from the one analyzed here.

Citation: Walonoski J, et al. Synthea: An approach, method, and software mechanism for generating synthetic patients and the synthetic electronic health care record. JAMIA. 2018;25(3):230–238.
