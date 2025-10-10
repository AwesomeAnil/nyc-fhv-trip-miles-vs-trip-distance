![Banner](/assets/elasticity_banner.png)

# 🧭 NYC FHV Fare Elasticity — Full Analysis Flow  
*A backward-looking investigation of fare determinants in New York City’s For-Hire Vehicle (FHV) industry.*

---

## 📘 1 · Purpose & Scope
This document details the full analytical workflow used to evaluate how **trip distance** and **trip time** influence the **base passenger fare**, and whether those relationships differ for **short** vs **long** trips.  

The goal is interpretive — to explain historical fare behavior, not predict future fares.

---

## 🧱 2 · Environment & Platform
Developed on **Microsoft Fabric (trial capacity)** using:

| Component | Purpose |
|------------|----------|
| **OneLake** | Centralized data lake for raw & processed trip data |
| **Delta Lake** | ACID-compliant storage for consistent tables |
| **Fabric Notebooks** | Interactive environment for EDA & modeling |
| **Data Pipelines** | Automated ingestion + transformation |

Local reproducibility is provided through a trimmed dataset in `sample_data/`.

---

## 🧹 3 · Data Preparation & Quality Checks
1. **Ingest** raw parquet files (`sample_data/source_*.parquet`).  
2. **Validate columns:** `trip_miles`, `trip_time`, `base_passenger_fare`.  
3. **Filter:** remove zero/negative or missing observations.  
4. **Persist:** write to Delta Lake table (Fabric) or CSV sample (local).  

**Business rationale:** ensures clean, auditable inputs; eliminates spurious fare entries.

---

## 📊 4 · Exploratory Data Analysis (EDA)
**Actions**
- Compute descriptive stats → `reports/summary_stats.csv`.  
- Visualize distributions and scatterplots.

**Visual placeholders**
- ![Scatter plots and Correlations](/images/scatter_correlations.png)


**Findings**
- Distance and time are positively correlated (~0.81).  
- Miles–fare correlation 0.86 > time–fare 0.79 ⇒ distance more influential.

---

## 💵 5 · Baseline Regression (Levels Model)

**Model (Linear Form)**  

\[
Fare = α + β_1 \, Miles + β_2 \, Time + ε
\]


| Term | Coefficient | Interpretation |
|------|------------|----------------|
| Intercept | 4.6515 | Base fare when distance & time = 0 |
| Miles | 2.3187 | Fare change per additional mile |
| Time | 0.4601 | Fare change per additional minute |

**Key Insights**  
- Distance is the dominant driver (2.32 vs 0.46 per unit).  
- Duration matters, especially for longer trips.  
- Baseline fare exists even for zero-mile trips.  
- Simple linear model captures trends but not elasticities or non-linear effects.
- ![Quick Initial Regression](/images/quick_regression.png)

---

## 📈 6 · Elasticity Regression (Log–Log Model)

**Model**  

log(Fare_i) = α + β_miles * log(Miles_i) + β_time * log(Time_i) + ε_i


| Term | Coefficient | Interpretation |
|------|------------|----------------|
| α | Intercept | Baseline log-fare |
| log(Miles) | 0.342 | Elasticity of fare w.r.t distance |
| log(Time) | 0.379 | Elasticity of fare w.r.t trip duration |

**Key Insights**  
- Fares scale elastically with both distance and time.  
- Slightly higher sensitivity to time than distance.  
- Model explains ~75.9% of variation in log-fare (R² ≈ 0.759).
- ![log-log regression](/images/log_regression.png)
- ![Elasticities](/images/elasticities_comparison.png)

---

## ⚖️ 7 · Elastic Regression by Distance Bins

| Distance Bin | Miles Elasticity | Time Elasticity | Dominant Driver |
|--------------|----------------|----------------|----------------|
| 0–2 mi | 0.04 | 0.41 | Time |
| 2–5 mi | 0.26 | 0.51 | Time |
| 5–10 mi | 0.47 | 0.43 | Balanced |
| 10–20 mi | 0.69 | 0.34 | Distance |
| 20–50 mi | 0.87 | 0.22 | Distance |
| 50+ mi | 0.87 | 0.02 | Distance |

**Key Insights**  
- Time elasticity falls as trip distance increases.  
- Distance elasticity rises sharply for longer trips.  
- Fare dynamics transition from time-dominant (<5 mi) to distance-dominant (>10 mi).
- ![Elasticities table by distance segments](/images/table_elasticities_distance_bins.png)

---

## 🚦 8 · Elastic Regression of Short vs. Long Trips (Interaction Model)

**Model**  

log(Fare_i) = beta_0 + beta_1 * log(Miles_i) + beta_2 * log(Time_i) + beta_3 * LongDummy
              + beta_4 * (log(Miles_i) * LongDummy) + beta_5 * (log(Time_i) * LongDummy) + ε_i


| Term | Coefficient | Interpretation |
|------|------------|----------------|
| Intercept | 1.499 | Baseline log-fare for short trips |
| log(Miles) | 0.179 | Distance elasticity for short trips |
| log(Time) | 0.440 | Time elasticity for short trips |
| long_dummy | -0.608 | Intercept shift for long trips |
| log(Miles) × long_dummy | 0.533 | Additional distance elasticity for long trips |
| log(Time) × long_dummy | -0.120 | Reduction in time elasticity for long trips |

**Key Insights**  
- Distance elasticity rises sharply for long trips (0.18 → 0.71).  
- Time elasticity drops for long trips (0.44 → 0.32).  
- Confirms non-linear, trip-length-dependent fare dynamics.  
- Supports dual fare regimes: time-dominant short trips, distance-dominant long trips.

---

## 🧩 9 · Elastic Regression with >5 Miles Cut-off

**Model**  

log(Fare_i) = beta_0 
            + beta_1 * log(Miles_i) 
            + beta_2 * log(Time_i) 
            + beta_3 * LongDummy
            + beta_4 * (log(Miles_i) * LongDummy)
            + beta_5 * (log(Time_i) * LongDummy) 
            + ε_i


| Term | Coefficient | Interpretation |
|------|------------|----------------|
| Intercept | 1.499 | Base log-fare for short trips (≤5 mi) |
| log(Miles) | 0.179 | Miles elasticity for short trips |
| log(Time) | 0.440 | Time elasticity for short trips |
| long_dummy | -0.608 | Intercept adjustment for long trips (>5 mi) |
| log(Miles) × long_dummy | 0.533 | Miles elasticity increment for long trips |
| log(Time) × long_dummy | -0.120 | Time elasticity change for long trips |

**Key Insights**  
- Elasticity shifts strongly for trips >5 mi: distance dominates long trips.  
- Time contribution diminishes for long trips.  
- Negative long_dummy means lower base fare for long trips, offset by higher distance elasticity.  
- R² ≈ 0.785, strong explanatory power.  
- Interactions highly significant; fare sensitivity varies by trip length.
- ![regression with interaction](/images/regression_interaction.png)

---

## 🧮 10 · Diagnostics & Robustness Checks
- Checked for outliers and heteroskedasticity.  
- Re-estimated with robust (HC3) SEs → results consistent.  
- Multicollinearity (VIF ≈ 2.3 for miles/time) acceptable.  
- No material change in coefficients post trim.  

---

## 🧾 11 · Synthesis & Business Interpretation
| Insight | Implication |
|----------|-------------|
| Distance dominates fare formation | Supports distance-weighted pricing models |
| Elasticity increases for long trips | Two-tier pricing logic reflected in data |
| Time effect steady but secondary | Congestion adds incremental cost |
| Robust model fit (R² ≈ 0.785) | High explanatory power |

---

## 💡 12 · Communication Artifacts
**Deliverables for stakeholders**
- Executive summary slide → `docs/presentation.md`  
- Visuals → `images/` (fare vs distance, elasticities, interactions)  
- Tables → `reports/` (summary stats, coefficients)  
- README overview + this analysis flow → ensures full reproducibility

---

## 🧩 13 · Optional Extensions
- Segment elasticities by **borough** or **time-of-day**.  
- Add **driver pay** or **surge multiplier** features.  
- Automate **Fabric pipeline** refreshes to track elasticity drift.

---

## 🏁 14 · Conclusion (Summary Narrative)
> **Distance is the principal determinant of NYC FHV base fares.**  
> Time plays a secondary role.  
> The distance–fare relationship steepens beyond ≈ 5 miles, revealing an implicit tiered pricing structure.  
> Findings are consistent with operational logic and provide a robust framework for regulatory and strategic decision-making.

---

### 🧠 Next Steps
1. Upload final plots to `images/`.  
2. Push summary tables to `reports/`.  
3. Link from README and `docs/presentation.md`.  
4. Tag `v1.0` release once visuals and tables are in place.

---

**License:** MIT | **Maintainer:** Anil · [`github.com/<yourusername>`](https://github.com/<yourusername>)
