# 🤝 Contributing to NYC FHV Fare Elasticity

Thank you for your interest in contributing!  
This repository combines open data, reproducible analytics, and public-good research.  
We welcome ideas, improvements, and collaborations from data scientists, engineers, and policy professionals.

---

## 🌱 How You Can Contribute

### 🧠 1. Research Enhancements
- Extend elasticity analysis by **borough**, **hour of day**, or **day type** (weekday vs weekend).  
- Test new econometric specifications (e.g., **robust regression**, **mixed effects**, **spatial models**).  
- Incorporate additional variables (e.g., driver pay, surge multiplier, congestion index).

### 💻 2. Code & Tooling Improvements
- Improve notebook readability or modularize into Python scripts.  
- Add data validation and profiling scripts under `/notebooks/` or `/sample_data/`.  
- Enhance visualization using **Plotly**, **Altair**, or **Seaborn** interactive dashboards.

### 🧾 3. Documentation & Design
- Refine `README.md` or `docs/presentation.md`.  
- Add diagrams or improve figures in `/images/`.  
- Translate summaries into other languages for accessibility.

---

## 🧰 Local Setup
```bash
# 1. Fork and clone
git clone https://github.com/<yourusername>/nyc-fhv-fare-elasticity.git
cd nyc-fhv-fare-elasticity

# 2. Create environment
conda env create -f environment.yml
conda activate fhv_fare_env

# 3. Run notebook
jupyter notebook notebooks/01_fare_elasticity.ipynb
```
For small-scale testing, use the preloaded sample_data/ folder.

---

## 🧪 Testing

If you add code (e.g., helper functions), please include lightweight tests:

pytest tests/

For notebooks, ensure they execute fully with no errors before submitting a PR.

---

## 📸 Visual Assets

All charts and figures belong in /images/.
Please ensure:

.png or .svg formats (no proprietary file types).

1080px minimum width for clarity on GitHub Pages.

Descriptive file names (e.g., elasticity_bar_plot.png).

---

## 🔍 Pull Request Guidelines

Each PR should address one issue or improvement.

Reference related issues in the PR description.

Include updated visuals/tables if analytical results change.

Keep commits concise and descriptive.

---

## ⚙️ Code of Conduct

All contributors are expected to follow the project’s Code of Conduct
.
We foster an inclusive, collaborative, and respectful environment for all participants.

---

## 🏆 Acknowledgement

Your contributions directly advance open, explainable, and fair mobility pricing research.
Thank you for helping make data science matter for cities.
