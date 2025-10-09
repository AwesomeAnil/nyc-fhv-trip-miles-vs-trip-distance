![Banner](/assets/elasticity_banner.png)

# 🧭 Executive One-Pager  
### 🚕 *NYC For-Hire Vehicle (FHV) Fare Elasticity — Understanding What Drives Urban Fares*

---

## 🚖 Project Summary
New York City’s For-Hire Vehicle (FHV) industry generates **tens of millions of trips every month**.  
Riders, regulators, and operators all depend on one question:

> **What actually drives the fare — distance or time?**

This project provides the first **data-driven, transparent, backward-looking explanation**  
of how **trip distance** and **trip time** shape the **base passenger fare**.

---

## 💡 Why This Matters
| Stakeholder | Challenge | What This Study Delivers |
|--------------|------------|---------------------------|
| **Riders** | Unpredictable fares erode trust | Transparent evidence on how fares are formed |
| **Regulators** | Need for fairness and compliance oversight | Empirical validation of distance-based pricing logic |
| **Operators** | Optimize revenue vs. cost structure | Quantifiable elasticity metrics |
| **Investors / Policymakers** | Demand clarity on market efficiency | Data-backed evidence for rational pricing |

> *Fair pricing starts with clear data.*

---

## 📊 Key Insights
| Metric | Value | What It Means |
|:--|:--|:--|
| **Trips analyzed** | 20.4 million (June 2025) | Statistically representative NYC sample |
| **Dominant driver** | Distance | Main determinant of base fares |
| **Distance elasticity** | 0.71 | 1% more distance → 0.7% higher fare |
| **Time elasticity** | 0.32 | 1% more time → 0.32 % higher fare |
| **Breakpoint (5 miles)** | +71% distance sensitivity (> 5 miles) vs. 0.18% (< 5 miles) | Long trips priced more aggressively |
| **R² of model** | 0.79 | Strong explanatory power |

---

## 💬 Headline Finding
> **Distance dominates NYC FHV pricing — and its influence grows with longer trips.**

This indicates a **tiered fare structure** aligned with driver effort and operational economics.  
It reflects a market that rewards mileage more heavily than time,  
confirming that **fare policies are distance-weighted and economically rational**.

---

## 🏗️ Built on Microsoft Fabric
- **OneLake** — unified data lake for raw and processed trip data  
- **Delta Lake** — ACID tables ensure data integrity and reproducibility  
- **Fabric Notebooks** — for EDA, regression modeling, and visualization  
- **Pipelines** — automated ingestion and refresh cycles  
- **Output** — portable results, HTML notebook, and policy-friendly summaries

> *Enterprise-grade reproducibility, powered by Microsoft Fabric trial capacity.*

---

## 💰 Business & Policy Impact
| Impact Area | Takeaway |
|--------------|-----------|
| **Pricing Strategy** | Reinforces distance-based fare logic for operators |
| **Policy Regulation** | Enables data-backed fairness audits |
| **Investor Insight** | Quantifies elasticity for valuation models |
| **Public Transparency** | Simplifies how fares are explained to citizens |

> Turning raw trip data into policy-ready insight.

---

## 🚀 Roadmap Ahead
- Extend to **borough-level** and **time-of-day** analysis  
- Incorporate **surge** and **driver pay** metrics  
- Build **monthly dashboards** with Fabric pipelines  
- Develop **“Fair Fare Index”** for continuous monitoring  

---

## 📘 Credits
**Author:** Anil [`github.com/<yourusername>`](https://github.com/<yourusername>)  
**License:** MIT  
**Dataset:** NYC TLC FHV Trip Data — June 2025  
**Platform:** Microsoft Fabric (OneLake · Delta Lake · Notebooks · Pipelines)

---

> *A one-page story of how open data, modern infrastructure, and transparent science  
are helping NYC move toward fairer fares and smarter mobility policy.*
