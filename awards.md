---
layout: page
permalink: /awards/index.html
title: Framing a Prediction Problem
---

## Framing a Prediction Problem

### Prediction Task  
- **Type**: Binary classification  
- **Goal**: Predict whether a recipe will receive high or low ratings based on its nutritional health informations and complexity (`high rating` represented by having an average rating ≥ 4.5 stars).

### Response Variable  
- **`high_rating`** (Boolean) — True if the recipe’s avg_rating ≥ 4.5; otherwise False.  
- **Why**: 
  - Converting recipes' average rating to high/low rating classes can make our predicting result more intuitive.
  - Since ratings are mainly concentrated on 5 points, we think it is reasonable to define that recipes with high overall ratings have an average ratings of at least 4.5 points.

### Feature Selection Rationale  
We began with the full suite of complexity and nutrition metrics but pared down to those with the strongest, most interpretable signals:  
- **Complexity**: originally considered `minutes`, `n_steps`, and `n_ingredients`, but found that `n_steps` and `n_ingredients` tracked very closely with `minutes` (similar distributions and correlations). To avoid redundancy, we retained **`minutes`** alone.  
- **Nutrition**: Whether containing healthy-related tags provides some health information about the recipes. From the nutrition data, we selected features that, as observed in earlier analysis, appear to have certain patterns with average rating (listed below). 'total_fat', 'protein', and carbohydrates'  do not directly provide nutritional health information, since total fat may contain saturated fat (healthy) or unsaturated fat (unhealthy). Protein may come from fish that meet the omega-3 requirement, or from unhealthy processed meat or high-fat red meat. And carbohydrates are divided into high-quality carbohydrates and low-quality carbohydrates. Thus, they were omitted to at the prediction stage.

### Chosen Features Known at Prediction Time  
- **Complexity**  
  - `minutes` (Preparation time)  
- **Nutrition**  
  - `calories`  
  - `sugar`, `sodium` (PDV)
- **Meta Tag**  
  - `is_healthy` (Boolean derived from health-related tags)

### Evaluation Metric  
- **Primary metric:** **F₁-score**
  - **Class imbalance:** Only ~20 % of recipes cross the 4.5-star line; accuracy would be inflated by the majority class.  
  - **Balanced error cost:** We care about both *precision* (avoid over-hyping average dishes) and *recall* (don’t miss hidden gems). F₁ is the harmonic mean of the two and captures that trade-off.  
- **Secondary metrics:** Recall and precision are also dicussed respectively for the final model for more comprehensive interpretation.

### Time-of-Prediction Justification  
All chosen features exist the moment a recipe is published: preparation time, nutrition breakdown, and author-assigned tags. Excluding any data that arrives later ensures the prediction is feasible in real time and avoids future information leakage.  

---
