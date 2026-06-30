# Predicting and Understanding Diabetes Risk

Which health factors are most linked to diabetes, and can we use them to flag who's at risk? This project digs into CDC survey data to find out.

![Project poster summarizing the diabetes risk analysis](poster.png)

## The question

Diabetes often goes unnoticed until it's advanced. We wanted to know if a handful of common health indicators could help spot high risk earlier. So we asked: which features are most strongly associated with diabetes, and how well can a simple model predict it?

We went in expecting blood pressure, physical activity, and BMI to be the top three. We were partly right.

## What we found

The strongest signals were high blood pressure, general health, and BMI. Physical activity, to our surprise, barely mattered.

| Factor | Correlation with diabetes |
|---|---|
| High blood pressure | 0.37 |
| BMI | 0.30 |
| Physical activity | 0.12 |

Both the decision tree and the random forest landed around 73% test accuracy, so the simpler, more readable model held its own. Since the goal was understanding *which* factors matter (not squeezing out maximum accuracy), the decision tree was the right call.

## The data

We used the [CDC Diabetes Health Indicators Dataset](https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset), built from the 2015 CDC health survey. It has 253,680 responses across 21 features covering chronic conditions, lifestyle, healthcare access, and demographics.

It's worth noting the data is self reported and collected by phone survey, so there's real potential for bias and underreporting.

## How we did it

We cleaned the data (dropped prediabetic cases, balanced the classes, removed outliers with a custom function), then trained a decision tree at a few different depths. To find the best features, we wrote a small algorithm that picked 5 random features, trained a tree, and repeated it 1,000 times to see which combinations performed best. Finally we compared everything against a random forest as a benchmark.

## A caveat

This is a screening aid, not a diagnostic tool. It points to correlation, not causation, and at 73% accuracy it should support a clinician's judgment, never replace it.

## Run it yourself

```bash
git clone https://github.com/soph-shen/predictingdiabetes.git
cd predictingdiabetes
pip install -r requirements.txt
jupyter notebook diabetes_risk_analysis.ipynb
```

Built with Python, pandas, scikit-learn, and matplotlib.

## Credits

A team project by The Databetics: Sophia Shen, Meghan Howell, Sylvie Bon-Harper, and Jeslyn Pratiknjo, for DATA110. The full report is in [diabetes_project_full_report.pdf](diabetes_project_full_report.pdf).
