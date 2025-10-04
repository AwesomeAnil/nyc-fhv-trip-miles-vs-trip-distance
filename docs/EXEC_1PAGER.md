# 🚕 NYC FHV Fare Elasticity Analysis — Executive One-Pager

---

## 🏙️ Overview

**Purpose:**  
Discover what drives fare variation in NYC’s For-Hire Vehicle market—distance, time, or both—and translate insights into actionable strategies for operators, regulators, and business leaders.

**Project:**  
Analysis of 20.4 million January 2025 NYC FHV trips, leveraging Microsoft Fabric, Python, and advanced analytics.

![NYC Map Thumbnail](images/nyc_map_thumb.png)

---

## 📊 Key Findings

- **Distance & Time Both Matter:**  
  - 📈 **Miles vs Fare:** 0.86 correlation  
  - ⏱️ **Time vs Fare:** 0.79 correlation  
- **Elasticity (Sensitivity):**
  - **Linear Model:** Fare ~ Distance (2.32), Time (0.46)
  - **Log-Log Model:** Elasticity—Distance (0.34), Time (0.38)
- **Tipping Point:**  
  - <5 miles: Fare driven by time (congestion, idling)  
  - >5 miles: Distance dominates

![Elasticity by Trip Length](images/elasticity_by_bin.png)

---

## 📈 Strategic Insights

- 🔹 **Short Trips:** Time-based surcharges matter most
- 🔹 **Long Trips:** Distance-based pricing is optimal
- 🔹 **Structural Break:** Fare model changes above 5 miles

> _Visual: Line chart shows shift from time to distance sensitivity at the 5-mile mark_

---

## 🧭 Data & Methods

- **Source:** NYC TLC FHV trip data, Jan 2025 (20.4M trips)
- **Tech:** Microsoft Fabric, Python (pandas, seaborn, statsmodels)
- **Methods:** Cleaning, correlation, regression, elasticity by bin, formal hypothesis testing

![Workflow](images/analysis_workflow.png)

---

## 🚀 Next Steps

- Analyze **driver pay elasticity**
- Visualize elasticity by borough and time of day
- Integrate insights into operational dashboards

---

## 💼 Contact

**Anil “AJ” Jacob**  
Principal BI & Analytics Leader  
📧 [Aniljacobs@gmail.com](mailto:Aniljacobs@gmail.com)  
🌐 [GitHub](https://github.com/AwesomeAnil/nyc-fhv-trip-miles-vs-trip-distance)

---

## 📚 References

- [NYC TLC FHV Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
- [Full Project & Notebook](https://github.com/AwesomeAnil/nyc-fhv-trip-miles-vs-trip-distance)

---
