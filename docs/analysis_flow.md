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
- `images/eda_hist_trip_miles.png`  
- `images/eda_scatter_miles_fare.png`

**Findings**
- Distance and time are positively correlated (~0.81).  
- Miles–fare correlation 0.86 > time–fare 0.79 ⇒ distance more influential.

---

## 💵 5 · Baseline Regression (Levels Model)
Model form:  
\[
Fare = α + β_1 \, Miles + β_2 \, Time + ε
\]

**Outputs** → `reports/levels_coef_table.csv`

| Term | Coefficient | Std Err | Interpretation |
|------|--------------|----------|----------------|
| Miles | ≈ 2.32 | — | +$2.32 per mile holding time fixed |
| Time | ≈ 0.46 | — | +$0.46 per minute |
| R² | ≈ 0.76 | — | Strong fit |

**Business takeaway:** Distance changes explain most fare variance in absolute $ terms.

---

## 📈 6 · Elasticity Regression (Log–Log Model)
Model form:  
\[
\log(Fare) = α + β_1 \log(Miles) + β_2 \log(Time) + ε
\]

**Elasticities**

| Variable | Elasticity | Meaning |
|-----------|------------|----------|
| Miles | 0.70 | 1 % ↑ distance ⇒ 0.7 % ↑ fare |
| Time | 0.24 | 1 % ↑ time ⇒ 0.24 % ↑ fare |

**Visual placeholder:** `images/elasticity_bar.png`  

**Business insight:** Distance ≈ 3× more impactful than time.

---

## ⚖️ 7 · Elasticity Equality Test (Wald)
**Null Hypothesis (H₀):** β₁ = β₂  
→ **F ≈ 608, p < 0.001 → Reject H₀.**

**Interpretation:** Distance effect ≫ time effect, statistically and practically.

---

## 🚦 8 · Interaction / Structural Difference Test
Model form (5 mi cut-off):  
\[
\log(Fare) = α + β_1 \log(Miles) + β_2 \log(Time) + δ [\log(Miles) \times LongTrip]
\]
→ F ≈ 9.2 × 10⁵ (p ≈ 0).

**Result:** Long-trip distance elasticity ↑ (≈ 0.34 vs 0.19).  

**Business meaning:** For trips > 5 mi, each % increase in distance yields greater fare increase.

---

## 🧩 9 · Step 6A — Extended Interaction Model (Pooled with Main + Interaction Terms)
Full form:  
\[
\log(Fare) = α + β_1\log(Miles) + β_2\log(Time) + γ LongDummy + δ_1(\log(Miles)\times LongDummy) + δ_2(\log(Time)\times LongDummy)
\]

**Key results** → `reports/interaction_coefs.csv`

| Term | Estimate | Interpretation |
|-------|-----------|----------------|
| β₁ log_miles | 0.189 | Short-trip distance elasticity |
| β₂ log_time | 0.427 | Short-trip time elasticity |
| γ long_dummy | 0.075 | Baseline fare shift (long trips) |
| δ₁ interaction | 0.154 | Extra distance elasticity for long trips (→ 0.34 total) |
| δ₂ interaction | −0.091 | Reduced time elasticity for long trips (→ 0.33 total) |
| R² | 0.82 | Improved fit |

**Findings**
- Distance elasticity rises from 0.19 → 0.34 for long trips.  
- Time elasticity drops slightly 0.43 → 0.33.  
- Intercept shift (+0.075) suggests higher base fare for long segments.

**Business rationale:** confirms tiered fare behavior aligned with operational pricing.

**Visual placeholders**
- `images/interaction_plot.png`  
- `images/coef_comparison.png`

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
| Robust model fit (R² ≈ 0.82) | High explanatory power |

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
