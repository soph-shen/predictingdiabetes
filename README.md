# Predicting and Understanding Diabetes Risk

Identifying the health factors most associated with diabetes — and predicting individual risk — using CDC survey data and decision-tree models.

> **TL;DR** Using 253,680 records from the CDC's Behavioral Risk Factor Surveillance System, I built decision-tree and random-forest classifiers to find which factors most strongly signal diabetes risk. **High blood pressure, general health, and BMI** came out as the strongest predictors — while physical activity, despite our initial hypothesis, did not.

![Project poster summarizing the diabetes risk analysis](assets/poster.png)

## Project Overview

Diabetes affects a growing share of the population, and many cases go unidentified until the condition is advanced. This project asks a practical question:

> **How can we identify individuals at high risk of diabetes earlier, by finding the features most common among diabetic individuals?**

Rather than chasing maximum accuracy, the goal was *interpretability* — finding the handful of factors a screening tool could focus on. Decision trees were chosen specifically because they rank features by importance and remain readable.

## Data

- **Source:** [CDC Diabetes Health Indicators Dataset](https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset) (derived from the 2015 CDC BRFSS survey)
- **Size:** 253,680 survey respondents, 21 features
- **Target:** diabetes status (non-diabetic vs. diabetic; prediabetic cases removed to sharpen the contrast)
- Features span chronic conditions, preventative health and lifestyle, healthcare access, general/mental health, and demographics.

## Approach

**Data preparation**
- Dropped prediabetic records to focus on a clean diabetic vs. non-diabetic split.
- Addressed class imbalance by randomly down-sampling non-diabetic records to match the diabetic group.
- Removed outliers from BMI, mental-health, physical-health, and age using a custom IQR-based function.

**Modeling**
- **Decision Tree Classifier** (scikit-learn), tested at depths 2–4 on an 80/20 train/test split.
- **Custom feature-selection algorithm:** randomly sampled 5 of 21 features, trained a depth-3 tree, and repeated **1,000 times** to surface the feature combinations that produced the best accuracy.
- **Random Forest** (`n_estimators=1000`, `max_depth=3`) as a benchmark for the interpretable tree.

## Key Findings

| Factor | Correlation with diabetes |
|---|---|
| High blood pressure (HighBP) | **+0.37** |
| BMI | **+0.30** |
| Physical activity | −0.12 |

- **HighBP, GenHlth (general health), and BMI** were consistently the most predictive features, appearing at the top of the trees and across feature-selection runs.
- **Physical activity was *not* a strong predictor** — contradicting our original hypothesis. Its weak correlation (−0.12) backed this up.
- Decision tree and random forest performed comparably (**~0.73 test accuracy**), confirming the simpler, interpretable model didn't sacrifice much — and gave us the feature ranking the random forest couldn't.

## Limitations

- The model is a **screening aid, not a diagnostic tool**, and does not establish causation.
- Source data is **self-reported** and collected via telephone survey (convenience sampling), introducing potential bias and underreporting.
- Accuracy is ~73%, so results should support — never replace — clinical judgment.

## Repository Contents

```
├── diabetes_risk_analysis.ipynb   # Main analysis notebook
├── diabetes.csv                   # Dataset
├── feature-description.csv        # Feature definitions
├── assets/poster.png              # Project summary poster
└── docs/                          # Full written report (PDF)
```

## How to Run

```bash
# 1. Clone the repo
git clone https://github.com/soph-shen/predictingdiabetes.git
cd predictingdiabetes

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch the notebook
jupyter notebook diabetes_risk_analysis.ipynb
```

## Tech Stack

Python · pandas · scikit-learn · matplotlib · Jupyter

---

*Originally developed as a team project ("The Databetics") for DATA110 — Xiyuan (Sophia) Shen, Meghan Howell, Sylvie Bon-Harper, and Jeslyn Pratiknjo. Full written report available in [`/docs`](docs/).*
