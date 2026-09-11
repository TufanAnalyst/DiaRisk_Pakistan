# DiaRisk_Pakistan

**Data Analytics & Machine Learning Project — End to End**

An end-to-end data analytics and machine learning project built on a real-world
clinical dataset collected from patients in Sahiwal, Pakistan. The project covers
data decoding, exploratory analysis, an interactive Power BI dashboard, and a
leakage-free machine learning model for early diabetes risk screening.

## 1. Project Overview

This project answers two distinct questions using the same dataset:

1. **Descriptive** — What does diabetes prevalence look like across age, gender,
   BMI, blood pressure, and symptoms in this patient population? *(Dashboard)*
2. **Predictive** — Can diabetes risk be flagged using only information a person
   can self-report at home, with no lab tests? *(ML Model)*

## 2. Dataset

**Source:** [Pakistani Diabetes Dataset on Kaggle](https://www.kaggle.com/datasets/mshoaibishaaq/pakistani-diabetes-dataset)

| Detail | Description |
|---|---|
| Diabetic records | 486 patients, District Headquarter Teaching Hospital (DHQTH), Sahiwal |
| Non-diabetic records | 426 participants, public medical camp, COMSATS University, Sahiwal Campus |
| Total records | 912 |
| Attributes | 18 clinical/demographic features + 1 target (`Outcome`) |

## 3. Data Decoding & Feature Engineering

Raw coded columns (`Gender`, `Rgn`, `his`, `vision`, `dipsia`, `uria`, `neph`,
`Outcome`) were decoded into human-readable labels. Additional engineered
features include `Age_Group`, `BMI_Category` (Asian population cutoffs),
`BP_Category` (full AHA staging including Hypertensive Crisis), `A1C_Category`,
and `Glucose_Category`. Full logic is documented in the project notebook.

## 4. Interactive Dashboard (Power BI)

A four-page dashboard covering:
- **Executive Overview** — prevalence by age, gender, BMI, region
- **Clinical Risk** — blood pressure staging, diagnostic marker distribution
- **Symptoms & Lifestyle** — symptom burden vs. outcome, activity level
- **Patient Explorer** — filterable drill-down table

See `dashboard/diabetes_dashboard.pbix`.

## 5. Machine Learning Model

### Data Leakage Investigation

Initial models using `A1C`, `Random_Glucose`, and `Duration_Years` achieved
100% test accuracy. Investigation via crosstab analysis confirmed these are
diagnostic/derived values rather than independent predictors — for example,
`Random_Glucose ≥ 200 mg/dL` is itself a clinical diabetes threshold, and
`Duration_Years = 0` for effectively all non-diabetic records. These fields
were excluded to build a genuine **early risk screening tool** rather than a
model that simply restates a lab diagnosis.

### Feature Pruning

Features were pruned in stages — first removing leakage variables, then
removing fields that require medical testing or prior diagnosis (`HDL`,
`Nephropathy`) to keep the model realistically self-reportable, then removing
statistically weak/negative-importance features (`Diastolic_BP`,
`Blurred_Vision_Label`, `Region_Label`) identified through permutation
importance across all five candidate models. The full before/after feature
list is documented in `features and model evaluation.txt`.

### Final Feature Set (self-reportable, no lab tests required)

```
Age, BMI, Waist_cm, Systolic_BP, Exercise_Minutes,
Gender_Label, Family_History_Label, Polydipsia_Label, Polyuria_Label
```

### Models Evaluated

Logistic Regression, Random Forest, XGBoost, MLP Neural Network, and SVC were
compared using **5-fold stratified cross-validation** on accuracy, precision,
recall, and F1 score. Feature importance was validated via permutation
importance across all five models to confirm consistency of signal.

**Final model: Support Vector Classifier (SVC)**

| Accuracy | Precision | Recall | F1 |
|---|---|---|---|
| 0.9396 | 0.9244 | 0.9666 | 0.9448 |

Full cross-validation comparison, confusion matrix, and feature importance
results are documented in `features and model evaluation.txt` and visualized
in `images/`.

## 6. Repository Structure

```
DiaRisk_Pakistan/
│
├── dashboard/
│   └── diabetes_dashboard.pbix
│
├── notebook/
│   └── Pak_diabetes_project.ipynb
│
├── images/
│   ├── background.png
│   ├── confusion_matrix.png
│   └── feature_importance.png
│
├── features and model evaluation.txt
└── README.md
```

## 7. Notebook

[Pak_diabetes_project.ipynb](https://github.com/TufanAnalyst/DiaRisk_Pakistan/blob/main/notebook/Pak_diabetes_project.ipynb)

## 8. Tech Stack

Python, pandas, scikit-learn, XGBoost, matplotlib, seaborn, Power BI

## 9. Author

**Ahmad Munir**
End-to-End Data Analytics & Machine Learning Project
