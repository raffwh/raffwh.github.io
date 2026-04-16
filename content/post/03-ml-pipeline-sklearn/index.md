---
title: "ML Pipeline: Predicting with Scikit-Learn"
description: End-to-end machine learning workflow including feature engineering, cross-validation, and model comparison — from graduate coursework.
slug: ml-pipeline-sklearn
date: 2026-03-01
image: ""
categories: "Projects"
toc: true
tags: [
    Python,
    scikit-learn,
    machine_learning,
    feature_engineering,
    cross_validation
]
---
## Overview

This post documents an end-to-end supervised machine learning workflow built in Python during my graduate coursework at Boston University. The goal was to build a reproducible pipeline that handles data preprocessing, feature selection, model training, and evaluation — the kind of workflow that translates directly to real-world analytics problems.

---

## Problem & Dataset

The analysis used a structured tabular dataset with a mix of numeric and categorical features. The target variable was a binary classification outcome. The challenge was to:

1. Handle missing values and skewed distributions thoughtfully
2. Engineer features that improve signal without data leakage
3. Select a modeling strategy that generalizes well

---

## Pipeline Design

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.model_selection import StratifiedKFold, cross_val_score
from sklearn.feature_selection import SelectKBest, f_classif

# Numeric and categorical transformers
numeric_transformer = Pipeline(steps=[
    ('scaler', StandardScaler())
])

categorical_transformer = Pipeline(steps=[
    ('encoder', OneHotEncoder(handle_unknown='ignore'))
])

# Column transformer
preprocessor = ColumnTransformer(transformers=[
    ('num', numeric_transformer, numeric_features),
    ('cat', categorical_transformer, categorical_features)
])

# Full pipeline with feature selection + model
pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('selector', SelectKBest(score_func=f_classif, k=15)),
    ('classifier', GradientBoostingClassifier())
])
```

---

## Cross-Validation & Model Comparison

Models were compared using **Stratified K-Fold cross-validation** (k=5) to account for class imbalance and reduce variance in performance estimates.

| Model               | CV AUC (mean ± std) |
| ------------------- | -------------------- |
| Logistic Regression | 0.81 ± 0.03         |
| Random Forest       | 0.87 ± 0.02         |
| Gradient Boosting   | 0.89 ± 0.02         |

```python
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(pipeline, X, y, cv=cv, scoring='roc_auc')
print(f"AUC: {scores.mean():.3f} ± {scores.std():.3f}")
```

---

## Feature Importance

After fitting the final Gradient Boosting model, feature importances were extracted and visualized using `matplotlib`. The top features aligned well with domain knowledge, providing a sanity check on the model.

---

## Key Takeaways

- **Pipeline design matters**: keeping preprocessing inside a `Pipeline` object prevents data leakage during cross-validation, a subtle but critical mistake to avoid.
- **Model selection**: Gradient Boosting outperformed simpler models, but the gain wasn't free — it required careful regularization to avoid overfitting on the training set.
- **Feature selection**: Using `SelectKBest` inside the pipeline reduced noise features and slightly improved generalization.

---

## Tools Used

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white)

**Full code available in:** [BU_OMDS_SU25_DX699B_RW](https://github.com/raffwh/BU_OMDS_SU25_DX699B_RW)
