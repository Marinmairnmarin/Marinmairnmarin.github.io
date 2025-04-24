---
layout: page
title: Baseline Model
permalink: /hobbies/index.html
css: "/assets/css/custom.css"
---

# Baseline Model for High-Rating Recipe Prediction

## Model Type
The baseline model is a **`DecisionTreeClassifier`** from *scikit-learn*.  
A decision tree offers a fast, easily interpretable starting point for classification.

## Features Used
| Feature          | Type          | Notes                                                |
|------------------|--------------|------------------------------------------------------|
| `minutes`        | Quantitative | Total cooking time                                   |
| `sugar`          | Quantitative | Grams of sugar (per-serving daily-value %)           |
| `sodium`         | Quantitative | Milligrams of sodium (per-serving daily-value %)     |
| `calories`       | Quantitative | Calories (per-serving)                               |
| `is_healthy`     | Nominal      | Boolean flag from health-related recipe tags         |

*No ordinal features* were included in this baseline.

## Feature Processing
- **Quantitative** features were used *as-is* (no scaling needed for trees).  
- **Nominal** feature `is_healthy` was already Boolean (0/1), so no encoding step was required.  
- All preprocessing and model fitting were wrapped in a single `sklearn.Pipeline`.

## Data Cleaning & Filtering
1. **Merged** raw *recipes* and *ratings* tables; removed rows with missing or zero ratings.  
2. **Dropped outliers** where any nutrient percentage exceeded 150 % of daily value.  
3. **Kept only recipes** with **> 3 user ratings** to ensure representative averages.

Resulting dataset: **≈ 13 878 recipes**.

## Prediction Task
- **Target variable:**  
  `high_rating` = True if `avg_rating` ≥ 4.5, else False.
- **Train / test split:** `train_test_split` with a fixed random seed (42).

## Performance
| Metric (test set) | Value |
|-------------------|-------|
| **F1 score**      | **0.836** |

The F1 score balances precision and recall and indicates strong baseline performance given the small feature set.

## Interpretation
- An F1 of ~0.84 suggests the tree reliably flags high-rated recipes while limiting false alarms.  
- **Strengths:** simplicity, interpretability, solid baseline to benchmark future work.  
- **Limitations / next steps:**  
  - Engineer richer features (ingredient vectors, detailed nutrition, textual reviews).  
  - Experiment with ensemble models (Random Forests, Gradient Boosting).  
  - Further refine outlier handling and class-balance strategies.

This baseline establishes a clear reference point for the upcoming **Final Model** in Step 5.
