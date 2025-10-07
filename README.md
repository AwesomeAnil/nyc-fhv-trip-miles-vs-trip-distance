# 🚕 NYC Taxi Fare Elasticity Analysis using Microsoft Fabric

![License](https://img.shields.io/badge/license-MIT-blue) ![Python](https://img.shields.io/badge/python-3.10%2B-green) ![Notebooks](https://img.shields.io/badge/notebooks-Jupyter-orange)

**One-line impact:** *Distance is the primary determinant of NYC FHV base fares; its influence strengthens for trips > 5 mi.*

---

## 📌 Key numbers (hero card)

* **Observations:** 20,405,666 trips (January 2025 sample — full dataset in original environment)
* **Elasticity (miles, log–log):** ~**0.70** (aggregate)
* **Marginal $/mile (levels model):** **~$2.32 / mile**
* **Segment change (>5 mi):** short-trip miles elasticity ≈ **0.19** → long-trip ≈ **0.34**

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
5. **Equality test (Wald)** — formal test: are elasticities equal?
6. **Interaction test (single breakpoint — e.g., 5 mi)** — does elasticity differ for long trips?
   **6A. Extended Interaction Model (pooled w/ interactions)** — unified model:
   log(Fare) = α + β₁log(Miles) + β₂log(Time) + γ·LongDummy + δ₁(log(Miles)×LongDummy) + δ₂(log(Time)×LongDummy)


* *Findings from notebook:* short-trip miles elasticity ≈ **0.19**, long-trip ≈ **0.34**; short-trip time ≈ **0.43**, long-trip ≈ **0.33**; intercept shift for long trips ≈ **+0.075**.

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
