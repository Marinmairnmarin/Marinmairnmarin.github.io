---
layout: page
permalink: /awards/index.html
title: Framing a Prediction Problem
---

## Step 3: Framing a Prediction Problem

### Prediction Task  
- **Type**: Binary classification  
- **Goal**: Predict whether a newly published recipe will receive a high average rating (≥ 4.5 stars).

### Response Variable  
- **`high_rating`** (Boolean) — True if the recipe’s avg_rating ≥ 4.5; otherwise False.  
- **Why**:  
  - The 1–5 rating scale is sharply skewed toward the high end; collapsing it into a hit / not-hit label yields a clearer, business-friendly question: *“Will this recipe delight users?”*  
  - A 4.5-star cutoff isolates roughly the top quintile of recipes, providing enough positive and negative cases for learning while marking a genuinely outstanding score.

### Feature Selection Rationale  
We began with the full suite of complexity and nutrition metrics but pared down to those with the strongest, most interpretable signals:  
- **Complexity**: originally considered `minutes`, `n_steps`, and `n_ingredients`, but found that `n_steps` and `n_ingredients` tracked very closely with `minutes` (similar distributions and correlations). To avoid redundancy, we retained **`minutes`** alone.  
- **Nutrition**: the raw data include seven PDV metrics (`total_fat`, `sugar`, `sodium`, `protein`, `saturated_fat`, `carbohydrates`, plus `calories`). We selected those (listed below) that showed clear trends with rating in our exploratory analysis. Metrics like `total_fat` can reflect both healthy (unsaturated) and unhealthy (saturated) fats and were omitted to simplify interpretation.  
- **See our website** for the full list of all features considered and detailed exploratory plots.

### Features Known at Prediction Time  
All predictors are available as soon as a recipe is published—no future “leaked” data:  
- **Complexity**  
  - `minutes` (total prep + cook time)  
- **Nutrition**  
  - `calories`  
  - `sugar`, `sodium`, `saturated_fat`, `carbohydrates`  
- **Meta Tag**  
  - `is_healthy` (Boolean derived from health-related tags)

### Evaluation Metric  
- **Primary metric:** **F₁-score** on the *high-rating* class  
  - **Class imbalance:** Only ~20 % of recipes cross the 4.5-star line; accuracy would be inflated by the majority class.  
  - **Balanced error cost:** We care about both *precision* (avoid over-hyping average dishes) and *recall* (don’t miss hidden gems). F₁ is the harmonic mean of the two and captures that trade-off.  
- **Secondary metrics:** Overall accuracy and ROC-AUC are reported for supplementary context but not used for model selection.

### Time-of-Prediction Justification  
All chosen features exist the moment a recipe is published: preparation time, nutrition breakdown, and author-assigned tags. Excluding any data that arrives later (user ratings, page views, etc.) ensures the prediction is feasible in real time and avoids future information leakage.  

---
