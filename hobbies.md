---
layout: page
permalink: /hobbies/index.html
title: Baseline Model
---

## Baseline Model for High-Rating Recipe Prediction

## Model Type
The baseline model is a **`DecisionTreeClassifier`** from *scikit-learn*.  
A decision tree offers a fast, easily interpretable starting point for classification.

## Features Used
- **4 Quantitative**: `minutes`, `sugar`, `sodium`, `calories`
- **1 Nominal**: `is_healthy`


## Feature Processing
- **Quantitative** features were used *as-is* (no scaling needed for trees).  
- **Nominal** feature `is_healthy` was already Boolean (0/1), so no encoding step was required.
  > _Note_: Decision trees split on raw values and do not require one-hot or label encoding. Other models (e.g., Logistic Regression, KNN) do require encoding of categorical inputs.  
- All preprocessing and model fitting were wrapped in a single `sklearn.Pipeline`.

## Performance

| Metric (test set) | Value |
|-------------------|-------|
| **F1 score**      | **0.848** |

The F1 score balances precision and recall and indicates strong baseline performance given the small feature set.

## Interpretation
From the value of the F1-score, this baseline model can be considered a reasonably good starting point, as it reliably identifies high-rated recipes. However, there is still substantial room for improvement—for instance, we have not yet conducted extensive data preprocessing or optimized the model’s hyperparameters. Thus, the result primarily suggests that the features we selected have predictive value for the rating class.
