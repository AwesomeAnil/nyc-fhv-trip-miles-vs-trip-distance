# NYC FHV Fare Elasticity: Trip Miles & Times vs. Trip Distance

Welcome to the **NYC FHV Fare Elasticity** project!  
This repository explores how trip distance and duration affect fare pricing for New York City's For-Hire Vehicles (FHV), using real trip data and statistical analysis.

---

## 🚀 Project Overview

**Goal:**  
Analyze how passenger fares are influenced by trip miles and trip times, uncovering the underlying fare elasticities in NYC's FHV market.

**Dataset:**  
- Over 20 million FHV trip records (Jan 2025)
- Each trip includes distance, duration, fare, and location info

**Audience:**  
- Non-technical: Taxi operators, policymakers, city planners, curious New Yorkers
- Technical: Data scientists, economists, urban researchers, ML engineers

---

## 📊 Key Insights

- **Trip miles** and **trip time** are both strong predictors of fare.
- **Correlation:** Miles vs Fare: **0.86**; Time vs Fare: **0.79**; Miles vs Time: **0.81**
- **Linear regression:** Distance (miles) is the strongest dollar predictor.
    - R²: **0.76** (combined model)
- **Log-log regression:** Time slightly surpasses distance in percentage terms.
    - Elasticity: log_miles ≈ **0.342**, log_time ≈ **0.379**
- **Elasticity shifts by trip length:**  
    - Short trips: Time dominates  
    - Long trips: Miles dominate  
    - Structural break for trips > **5 miles**
- **Formal tests:** Elasticities are significantly different, and their importance varies by trip length.

---

## 🛠️ Methodology

1. **Data Processing:**  
   - Loaded 20M+ trips using Spark and Pandas
   - Cleaned: Removed negative fares/driver pay, dropped columns with major nulls
2. **Exploratory Analysis:**  
   - Visualized trip miles, trip times, and fares
   - Correlations and distribution analysis
3. **Modeling:**  
   - Linear and log-log regression on fare vs. miles/time
   - Elasticities calculated across trip length bins
   - Wald tests for hypothesis validation
   - Visualization of elasticities and structural breaks

---

## 📈 Example Results

- **Correlation:**  
  - Miles vs Fare: **0.86**
  - Time vs Fare: **0.79**
- **Linear Model:**  
  - R²: **0.76** (fare ~ miles + time)
  - Coefficients: miles ≈ **2.32**, time ≈ **0.46**
- **Log-Log Model:**  
  - R²: **0.76** (log(fare) ~ log(miles) + log(time))
  - Elasticity: miles ≈ **0.342**, time ≈ **0.379**
- **Elasticity by Distance Bin:**
| Bin      | Miles Elasticity | Time Elasticity |
|----------|------------------|-----------------|
| 0-2 mi   | 0.04             | 0.41            |
| 2-5 mi   | 0.26             | 0.51            |
| 5-10 mi  | 0.47             | 0.43            |
| 10-20 mi | 0.70             | 0.34            |
| 20-50 mi | 0.89             | 0.23            |
| 50+ mi   | 0.70             | 0.24            |
- **Structural Break:**  
  - Miles elasticity overtakes time for trips > **5 miles**

---

## 🔬 Formal Hypothesis Tests

- **Elasticities for miles and time are not equal** (strongly rejected)
- **Elasticities differ by trip length** (strongly rejected)
- **Structural breaks** identified above 5 miles in trip distance

---

## 📂 Repository Structure

- `notebooks/01_Fare_Elasticity.html` — Full code, visualizations, and statistical outputs
- `Files/uploads/fhvhv_tripdata_2025-01.parquet` — Raw trip data (requires access)
- Other scripts and notebooks for data loading, analysis, and visualization

---

## 🧑‍💻 How to Run the Analysis (Technical Users)

1. **Clone the repo:**  
   ```bash
   git clone https://github.com/AwesomeAnil/nyc-fhv-trip-miles-vs-trip-distance.git
   ```
2. **(Optional) Download Data:**  
   Place the FHV trip parquet file in `Files/uploads/`.
3. **Install requirements:**  
   - Python 3.8+
   - pandas, numpy, matplotlib, seaborn, statsmodels, pyspark
   ```bash
   pip install pandas numpy matplotlib seaborn statsmodels pyspark
   ```
4. **Run the notebook:**  
   Open `notebooks/01_Fare_Elasticity.html` or the Python notebook version in Jupyter or VS Code.

---

## 📚 For Non-Technical Users

- **No setup required!**  
  Just view the HTML notebook for interactive charts and explanations.
- **Visual Summaries:**  
  - Histograms of trip miles and times
  - Scatterplots and regression lines for fare analysis
  - Elasticity charts across trip bins

---

## 🌇 Why Does This Matter?

Understanding fare elasticity:
- Helps optimize fare policies for fairness and efficiency.
- Aids drivers and riders in predicting trip costs.
- Informs city planners about congestion and pricing strategies.

---

## 🤝 Contributing

- Pull requests welcome!
- Share feedback, propose new analyses, or suggest improvements.

---

## 📝 License

MIT License.  
See [LICENSE](LICENSE) for more information.

---

## 📬 Contact

Questions, ideas, or collaborations?  
**GitHub:** [AwesomeAnil](https://github.com/AwesomeAnil)

---

## 📎 References

- NYC TLC FHV Trip Data: [Official site](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
- [Notebook HTML](https://github.com/AwesomeAnil/nyc-fhv-trip-miles-vs-trip-distance/blob/main/notebooks/01_Fare_Elasticity.html)