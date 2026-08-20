# Hydroforming Operations Analysis

An operational analysis of a three-month hydroforming production report, focused on machine continuity, interruptions, throughput, scrap, and job transitions.

The repository includes two finished notebooks:

- **Concise summary:** a focused review of the strongest findings and recommended actions.
- **Detailed analysis:** the complete analytical flow, supporting queries, visualizations, interpretations, and limitations.

## Analysis sequence

The analysis moves from production context to targeted diagnosis:

1. **Validate the production structure.** Establish the report, job, product, and machine relationships, then sequence reports within each machine timeline.
2. **Measure continuity.** Check overlaps and quantify the time between consecutive reports. No reports overlap; for gaps below 40 minutes, the median gap is **8.24 minutes**, and only **10.7%** of positive gaps coincide directly with a job change.

   ![Distribution of production gaps below 40 minutes](image/Distribution%20of%20Production%20Gaps%20Under%2040%20Minutes.png)

3. **Locate interruption exposure.** Compare interruption minutes with total reported time by product–machine combination, separating recurring high-volume losses from combinations with limited exposure.

   ![Interruption impact by product and machine](image/Interruption%20Impact%20by%20Product%20and%20Machine.png)

4. **Compare operating performance.** Review observed throughput for the same product across machines to identify combinations that merit process investigation; the comparison is descriptive and does not by itself establish machine causality.

   ![Observed throughput by product and machine](image/Observed%20Throughput%20by%20Product%20and%20Machine.png)

5. **Track weekly machine stability.** Aggregate interruption and scrap numerators and denominators before calculating weekly rates. This reveals whether poor performance is persistent or concentrated in specific weeks.

   ![Weekly scrap rate by machine](image/Weekly%20Scrap%20Rate%20by%20Machine.png)

6. **Drill into the main scrap signal.** Device 169 is the primary concern: Product 297 records about **7.63% scrap**, versus approximately **0.93%–1.15%** on the other machines where it runs. The issue is concentrated in weeks 26–29, when its weekly scrap rate is approximately **6.15%–11.78%**.

   ![Weekly scrap rate for Product 297 by machine](image/Weekly%20Scrap%20Rate%20for%20Product%20297%20by%20Machine.png)

7. **Test the job-transition signal.** Compare reports immediately before and after a job change with normal production. The first report after a change has a higher median interruption duration on all five machines.

   ![Interruption duration around job changes by machine](image/Interruption%20Duration%20Around%20Job%20Changes%20by%20Machine.png)

The sequence narrows broad operational patterns into two investigation priorities: the Device 169/Product 297 scrap issue and elevated interruption time immediately after job changes. These are diagnostic leads, not confirmed causes.

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

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── hydroforming_operations_analysis_concise_summary.ipynb
├── hydroforming_operations_analysis_detailed.ipynb
├── image/
│   └── supporting analysis figures
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

## Limitations

- The analysis covers approximately three months.
- The dataset does not include shift, operator, material batch, defect category, maintenance event, or interruption reason.
- The first and last calendar weeks are partial weeks.
- Estimated runtime assumes interruption minutes are measured consistently and can be subtracted from report duration.
- Results are observational and should be used to prioritize investigation, not to claim a confirmed cause.

## Tools

Python, pandas, NumPy, SQLite, and Plotly.
