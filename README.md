# Hydroforming Operations Analysis

An operational analysis of a three-month hydroforming production report, focused on machine continuity, interruptions, throughput, scrap, and job transitions.

The repository includes two finished notebooks:

- **Concise summary:** a focused review of the strongest findings and recommended actions.
- **Detailed analysis:** the complete analytical flow, supporting queries, visualizations, interpretations, and limitations.

## Business questions

The analysis addresses six practical questions:

1. What do the main report, job, product, and machine identifiers represent?
2. How continuous is production, and are gaps mainly linked to job changes?
3. Which product–machine combinations lose the most reported time to interruptions?
4. Does observed throughput for the same product differ across machines?
5. Which machines and weeks have the highest interruption and scrap rates?
6. Is interruption time higher around job transitions?

## Dataset

| Measure | Coverage |
|---|---:|
| Production reports | 2,793 |
| Jobs | 153 |
| Products | 30 |
| Machines | 5 |
| Reporting period | June–early September 2023 |

One row represents one production report. Each job is associated with one product and one machine, while the same product can run under other jobs and on other machines.

Machine IDs used in the report:

| Device ID | Machine |
|---:|---|
| 168 | Hydroforming 9 |
| 169 | Hydroforming 5 |
| 170 | Hydroforming 8 |
| 172 | Hydroforming 4 |
| 321 | Hydroforming 12 |

## Key findings

- Production reports do not overlap within a machine timeline. Among gaps below 40 minutes, the median is **8.24 minutes**.
- Most positive gaps occur within the same job. Only **10.7%** are directly paired with a job change.
- Interruption and observed throughput differ across product–machine combinations. Low-exposure combinations need to be separated from repeated, high-volume losses.
- **Device 169 is the main scrap concern.** Product 297 records about **7.63% scrap** on Device 169, compared with approximately **0.93%–1.15%** on the other machines where it runs.
- The Device 169/Product 297 issue is concentrated in weeks 26–29, when weekly product scrap ranges from approximately **6.15% to 11.78%**.
- The **first report after a job change** has a higher median interruption time than normal production on all five machines.

These findings identify where operational investigation should begin. They do not establish causation from the available fields alone.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── hydroforming_operations_analysis_concise_summary.ipynb
├── hydroforming_operations_analysis_detailed.ipynb
└── data/
    ├── hydroforming-scamp-db-sqlite.db
    ├── scamp_devices.csv
    └── scamp_report_3m.csv
```

## How to run

Clone the repository and create a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

On Windows, activate the environment with:

```powershell
.venv\Scripts\activate
```

Open `hydroforming_operations_analysis_concise_summary.ipynb` for the highlight version or `hydroforming_operations_analysis_detailed.ipynb` for the complete analysis. Run the notebooks from the repository root because the SQLite connection uses the relative path `data/hydroforming-scamp-db-sqlite.db`.

## Method summary

- SQL window functions sequence reports within each machine and identify gaps and job transitions.
- Interruption impact is calculated from interruption minutes relative to total reported time.
- Weekly scrap and interruption rates aggregate their numerators and denominators before calculating rates.
- Product–machine throughput is compared descriptively using reported hours and production volume.
- Device 169 is isolated for a product-level scrap investigation, followed by a cross-machine comparison of Product 297.

## Limitations

- The analysis covers approximately three months.
- The dataset does not include shift, operator, material batch, defect category, maintenance event, or interruption reason.
- The first and last calendar weeks are partial weeks.
- Estimated runtime assumes interruption minutes are measured consistently and can be subtracted from report duration.
- Results are observational and should be used to prioritize investigation, not to claim a confirmed cause.

## Tools

Python, pandas, NumPy, SQLite, and Plotly.
