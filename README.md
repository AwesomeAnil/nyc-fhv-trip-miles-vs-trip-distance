# 🚕 NYC Taxi Fare Elasticity Analysis using Microsoft Fabric

![License](https://img.shields.io/badge/license-MIT-blue) ![Python](https://img.shields.io/badge/python-3.10%2B-green) ![Notebooks](https://img.shields.io/badge/notebooks-Jupyter-orange)

**One-line impact:** *Distance is the primary determinant of NYC FHV base fares; its influence strengthens for trips > 5 mi.*

---

## 📌 Key numbers (hero card)

* **Observations:** 500,000 trips (January 2025 sample — full dataset in original environment)
* **Elasticity (miles, log–log):** ~**0.3420** (aggregate)
* * **Elasticity (time, log–log):** ~**0.3787** (aggregate)
* **Marginal $/mile (levels model):** **~$2.3187 / mile**
* **Segment change (>5 mi):** short-trip miles elasticity ≈ **0.18** → long-trip ≈ **0.53**

---

## 📋 Executive Summary

This repo contains a **backward-looking explanatory analysis** of NYC For-Hire Vehicle (FHV) trip data. The goal is to quantify how **trip distance** and **trip time** explain the **base passenger fare**, and whether fare responsiveness changes for long trips.
**Primary takeaway:** distance drives fares more than time, and distance sensitivity increases for longer trips — actionable for pricing strategy, audits, and regulator reviews.

---

## 🎯 Objectives

* Measure the relative influence of **distance** and **time** on base fare.
* Provide interpretable metrics: **$/mile**, **$/minute**, and **elasticities**.
* Test whether fare structure differs for **short vs long trips** (interaction models).
* Deliver business-friendly communication materials and reproducible research artifacts.

---

## 🧭 Analytical Flow (concise)

> Full methodological breakdown is in `reports/analysis_flow.md` (placeholder).

1. **Data prep & quality checks** — validate `trip_miles`, `trip_time`, `base_passenger_fare`; remove invalid rows; small sample available in `sample_data/`.
2. **Exploratory Data Analysis** — summary stats, scatterplots, and correlations.
3. **Baseline regression (levels)** — quantify $/mile and $/time (interpretable to ops/finance).
4. **Elasticity regression (log–log)** — % responsiveness: miles vs time.
5. **Elasticity regression looped through distance bins** — plot elastcity of miles vs time across segmented distances.
6. **Equality test (Wald)** — formal test: are elasticities equal?
7. **Interaction test (single breakpoint — e.g., 5 mi)** — does elasticity differ for long trips?
   **6A. Extended Interaction Model (pooled w/ interactions)** — unified model:
   log(Fare) = α + β₁log(Miles) + β₂log(Time) + γ·LongDummy + δ₁(log(Miles)×LongDummy) + δ₂(log(Time)×LongDummy)


* *Findings from notebook:* short-trip miles elasticity ≈ **0.18**, long-trip ≈ **0.53**; short-trip time ≈ **0.44**, long-trip ≈ **-0.12**; intercept shift for long trips ≈ **+0.075**.

7. **Synthesis & communication** — dashboards, one-page executive summary, and reproducible artifacts.

---

## 📊 Visuals & Artifacts (placeholders)

> I left placeholders so you can add final PNGs / CSVs after re-running the final notebook.

**Important visuals**

* EDA scatter: `images/eda_scatter_miles_fare.png`
* Elasticities bar chart: `images/elasticity_bar.png`
* Levels regression coefficients table: `reports/levels_coef_table.csv`
* Interaction model coefficients: `reports/interaction_coefs.csv`
* Presentation slides (rendered): `docs/presentation.md` (or PDF in `docs/`)

*(Add your real PNGs to `images/` and CSVs to `reports/`.)*

---

## 🏗️ Built on Microsoft Fabric — high-level notes

This analysis was developed on **Microsoft Fabric** trial capacity and leveraged Fabric concepts for enterprise-grade reproducibility:

* **OneLake** — central storage for datasets and processed outputs (project-managed lake).
* **Delta Lake** — canonical table format used to ensure consistent results and incremental ETL semantics.
* **Fabric Notebooks** — interactive environment for EDA, modeling, and visual exports.
* **Data Pipelines** — Fabric pipelines orchestrated ingestion/transform for production runs (scheduling and refresh).

> **Reproducibility note:** This README includes a small `sample_data/` subset so you can reproduce the core notebook locally without a Fabric trial. If you want to re-run on Fabric, import data to OneLake and point the notebook to your workspace.

---

## 🧩 Repo structure (recommended)

```
nyc-fhv-fare-elasticity/
├── notebooks/ ← Jupyter notebooks (analysis & models)
│ ├── 01_fare_elasticity.ipynb
│ └── 01_fare_elasticity.html ← rendered notebook (optional)
├── reports/ ← CSVs and regression summary tables
│ ├── summary_stats.csv
│ ├── levels_coef_table.csv
│ └── interaction_coefs.csv
├── docs/ ← Presentation, CONTRIBUTING & CODE_OF_CONDUCT placeholders
│ ├── presentation.md
│ ├── CONTRIBUTING.md
│ └── CODE_OF_CONDUCT.md
├── images/ ← PNGs, screenshots, figure assets (placeholders)
│ ├── eda_scatter_miles_fare.png
│ └── elasticity_bar.png
├── sample_data/ ← small reproducible sample (source & transformed)
│ ├── sample_1k.parquet
│ └── sample_transformed_1k.parquet
├── README.md
├── requirements.txt
└── LICENSE (MIT)

```

---

## 🛠️ Quickstart — run locally (using sample_data)

```bash
# 1. Clone
git clone https://github.com/<yourusername>/nyc-fhv-fare-elasticity.git
cd nyc-fhv-fare-elasticity

# 2. Install (recommended virtualenv)
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# 3. Open the notebook (works with sample_data)
jupyter notebook notebooks/01_fare_elasticity.ipynb
# or view rendered HTML
open notebooks/01_fare_elasticity.html

```
---

## 🤝 Contributing

We welcome contributions. To get started:

1. Fork the repo and create a branch: `feature/<your-idea>`
2. Make changes, add tests or a small notebook demonstrating your idea
3. Add or update artifacts in `images/` and `reports/` as needed (respect naming conventions)
4. Open a PR describing your change and link any updated notebook outputs

**Good first issues**

* Borough-level elasticity analysis (split by borough)
* Interactive Plotly version of the elasticity plot (in `images/` + `notebooks/`)
* Add unit/integration tests for data transformation script(s)

**Docs:** `docs/CONTRIBUTING.md` and `docs/CODE_OF_CONDUCT.md` are placeholders — please update with contribution rules and behavior guidelines when you're ready.

---

## 📦 Dependencies

Minimum dependencies (example; pin exact versions in `requirements.txt`):

* Python 3.10+
* pandas, numpy, statsmodels, matplotlib, jupyterlab, pyarrow

---

## 🧾 License

MIT License — see [`LICENSE`](LICENSE)

---

## 📫 Contact

Anil — `anil@example.com` · GitHub: `github.com/<yourusername>`

---

## 🏁 Closing Note

* Distance largely explains fare variation; time matters but less so
* Elasticities and interaction tests show distance becomes more important for long trips
* Repo is reproducible with `sample_data/`; enterprise execution used MS Fabric (OneLake + Delta + Notebooks + Pipelines)
* Add visuals to `images/` and tables to `reports/` after rerunning notebooks and push them to the repo



