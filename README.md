# 🚕 NYC Taxi Fare Elasticity Analysis using Microsoft Fabric

### 🧭 End-to-End Interpretive Analytics Workflow on Real-World Data
This project explores **New York City’s high-volume For-Hire Vehicle (FHV)** dataset to determine whether **trip distance** or **trip time** is a *stronger structural predictor* of **base passenger fares**.  
It builds a full **end-to-end analytics pipeline in Microsoft Fabric**, applying exploratory data analysis, log–log regression, and comparative elasticity modeling to derive interpretable insights.

---

## 📦 Project Overview

### 🎯 Objective
To quantitatively assess whether **trip distance** or **trip duration** better explains fare variation in NYC’s For-Hire Vehicle data, using **log–log regression** to measure **fare elasticity** and **model fit (R²)** as indicators of explanatory power.

This is an **interpretive and diagnostic analytics exercise**, not a fare prediction project.

### 🏗️ Tech Stack
- **Microsoft Fabric** — Lakehouse, Notebooks, Data Pipelines, ML environment  
- **Python** — `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`  
- **Delta Tables** — scalable, versioned data storage  
- **Power BI** — visualization and storytelling  

### 📂 Folder Structure
```
├── docs/ # showing PRESENTATION, EXEC_1Pager, Configuration markdowns
├── sample_data/ # sample aw NYC Taxi datasets including engineeerd data
├── images/ # screenshots of images on README and PRESENTATION markdowns.
├── notebooks/ # Fabric notebooks (EDA, Modeling, Prediction)
├── powerbi/ # Visual reports (PDF exports)
└── README.md # Project documentation
```

---

## 🔍 1. Exploratory Data Analysis (EDA) 🧮

### 🧰 Dataset Overview
- Source: NYC TLC FHV trip data (Jan–Jul 2025)
- Volume: ~20 million rows/month  
- Key Columns: pickup/dropoff times, trip distance, duration, total fare, payment type

### 📊 Suggested Plots
- Fare vs Distance scatter (linear + log scale)  
- Hourly pickup distribution heatmap  
- Correlation matrix of continuous features  

### 💡 Insights
- Distance shows the strongest positive correlation with fare.  
- Duration correlates moderately, especially in high-traffic hours.  
- Outliers (zero fares/distances) were removed pre-analysis.  

---

## 🧪 2. Feature Engineering ⚙️

### 🔧 Transformations
- Derived variables:
  - `trip_duration` (minutes)
  - `trip_speed` (mph)
  - `is_weekend`, `hour_of_day`
- Applied `log()` transforms to normalize continuous variables:
  - `log_fare`, `log_distance`, `log_duration`
- Filtered implausible values (distance < 0.1 or > 50 miles)

### 📊 Suggested Plots
- Feature distributions before/after log-transform  
- Boxplots for outlier detection  
- Correlation heatmap for engineered features  

### 💡 Outcome
A normalized, cleaned dataset ready for elasticity and regression analysis.

---

## ⚡ 3. Quick Regression Model (Baseline) 🧮

### 🎯 Objective
Establish a baseline fit between **fare amount** and **trip distance** using a simple linear regression.

### ⚙️ Model Specification
\[
\text{fare\_amount} = \alpha + \beta_1(\text{trip\_distance}) + \epsilon
\]

| Metric | Value |
|--------|------:|
| R² | 0.81 |
| RMSE | 2.95 |
| Coefficient (β₁) | 2.57 |

### 💡 Interpretation
- Fare increases roughly linearly with distance, but residuals widen at higher distances.  
- Indicates heteroscedasticity — variance increases with distance.  
- Motivates a **log–log model** to stabilize variance and measure elasticity.

### 🎨 Suggested Plots
- Fare vs Distance scatter + regression line  
- Residual vs fitted values plot  
- QQ plot for residual normality  

---

## 📏 4. Log–Log Regression by Trip Distance 📈

### 🎯 Objective
Quantify the **elasticity of fare with respect to trip distance** — how fare changes (%) for a 1% change in distance.

### ⚙️ Model Equation
\[
\log(\text{fare\_amount}) = \alpha + \beta_1 \log(\text{trip\_distance}) + \epsilon
\]

| Metric | Value |
|--------|------:|
| R² | **0.88** |
| RMSE (log scale) | 0.145 |
| β₁ (Elasticity) | **0.98** |

### 💡 Interpretation
- A 1% increase in distance yields ~0.98% increase in fare — near-proportional scaling.  
- High model fit (R² = 0.88) confirms **distance as the dominant structural driver** of fares.  

### 🎨 Suggested Plots
- `log(fare)` vs `log(distance)` scatter with regression line  
- Residual vs fitted plot  
- Distribution of `log(fare)`  

---

## ⏱️ 5. Log–Log Regression by Trip Duration 🕒

### 🎯 Objective
Assess whether **trip duration** explains fare variation as strongly as **trip distance**.

### ⚙️ Model Equation
\[
\log(\text{fare\_amount}) = \alpha + \beta_1 \log(\text{trip\_duration}) + \epsilon
\]

| Metric | Value |
|--------|------:|
| R² | 0.72 |
| RMSE (log scale) | 0.214 |
| β₁ (Elasticity) | **0.67** |

### 💡 Interpretation
- Duration elasticity (0.67) is substantially weaker than distance elasticity (0.98).  
- Suggests fares are primarily **distance-based**, with time effects contributing secondary adjustments (e.g., traffic, idle time).  

### 🎨 Suggested Plots
- `log(fare)` vs `log(duration)` regression line  
- Elasticity comparison bar chart  

---

## ⚖️ 6. Comparative Elasticity Analysis 📊

| Model | Predictor | Elasticity (β₁) | R² | RMSE (log) |
|--------|------------|----------------:|----:|-------------:|
| A | Distance | **0.98** | **0.88** | 0.145 |
| B | Duration | 0.67 | 0.72 | 0.214 |

### 💡 Key Insight
- **Distance elasticity** dominates across both magnitude and model fit.  
- **Trip distance** explains nearly **88% of fare variance**, compared to 72% for duration.  
- Indicates **fare structure is distance-driven**, with **time-based surcharges** playing a secondary role.

### 🎨 Suggested Plot
Side-by-side log–log scatterplots (distance vs duration), annotated with R² and elasticity values.

---

## 🧠 7. Interpretive Summary

| Dimension | Finding |
|------------|----------|
| Dominant Variable | **Trip Distance** (β₁ ≈ 0.98) |
| Secondary Variable | Trip Duration (β₁ ≈ 0.67) |
| Model Strength | R² = 0.88 for distance-driven model |
| Implication | Fare structure primarily scales with distance |
| Analytical Lens | Elasticity-based interpretive modeling (not prediction) |

---

## 📊 8. Power BI Visualization 💡

### 🧩 Dashboard Highlights
- **Fare vs Distance Elasticity** – regression trend visualization  
- **Distance vs Duration Elasticity Comparison** – dual bar chart  
- **Model Fit (R²)** comparison tiles  
- **Residual Analysis** – actual vs fitted log fares  

| Tile | Visualization | Insight |
|------|---------------|----------|
| Elasticity | Bar chart (β₁ comparison) | Distance dominates fare structure |
| Fit Metrics | Card visuals | R² comparison |
| Log–Log Trend | Scatter plot | Structural scaling confirmation |
| Residuals | Histogram | Homoscedasticity validation |

---

## 🧩 9. Key Takeaways

| Aspect | Insight |
|--------|----------|
| Analytical Type | Interpretive regression (elasticity modeling) |
| Key Question | Which variable better explains base passenger fares? |
| Answer | **Trip distance** is the stronger predictor (β₁=0.98 vs β₁=0.67) |
| Data Period | Jan–Jul 2025 |
| Broader Value | Quantifies structural dynamics of NYC fare system |

---

## 🚀 Conclusion

This analysis provides **quantitative proof** that NYC’s base passenger fares scale **nearly proportionally with trip distance**, while **trip time** contributes secondarily through surcharges or congestion effects.  

By leveraging **Microsoft Fabric** for reproducible analytics, this project demonstrates how **elasticity modeling** can transform large-scale mobility data into interpretable economic insights — bridging **data science, transportation economics, and business intelligence**.

> 🧭 “Predictive accuracy matters — but understanding *why* it happens defines real intelligence.”

---

## 🏁 Next Steps
- Test **interaction models** (`log(distance × duration)`)  
- Extend analysis to **driver pay elasticity**  
- Visualize elasticity shifts across boroughs and traffic conditions  

---

## ✍️ Author
**Anil “AJ” Jacob**  
Principal BI & Analytics Leader | Microsoft Fabric | Azure | Power BI | ML  
📍 Bangalore, India  
📧 [Aniljacobs@gmail.com](mailto:Aniljacobs@gmail.com)  
🌐 GitHub: [github.com/aniljacob](#) *(replace with repo link)*

---
