---
layout: page
permalink: /awards/index.html
title: Framing a Prediction Problem
---
## Step 3: Framing a Prediction Problem

### Prediction Task  
- **Type**: Regression  
- **Goal**: Predict a recipe’s **average user rating** on a 1–5 scale from its metadata and nutrition.

### Response Variable  
- **`avg_rating`** (continuous)  
- **Why**: We want fine‑grained predictions of user satisfaction rather than broad buckets, and `avg_rating` captures the mean of all non‑zero user ratings.

### Features Known at Prediction Time  
All predictors are available as soon as a recipe is published—no future “leaked” data:  
- **Complexity**  
  - `minutes` (prep time)  
  - `n_steps` (number of preparation steps)  
  - `n_ingredients` (ingredient count)  
- **Nutrition**  
  - `calories`  
  - `total_fat`, `sugar`, `sodium`, `protein`, `saturated_fat`, `carbohydrates`  

### Evaluation Metric  
- **Primary**: Root‑Mean‑Squared Error (RMSE)  
  - RMSE is in the same units as ratings (stars) and penalizes large errors more heavily, helping avoid egregious misestimates.  
- **Secondary**: R² score  
  - Reports the fraction of variance in `avg_rating` explained by our model.

---

> **Note on Alternatives**  
> If we instead framed this as a **binary classification** (e.g. Low Rating ≤ 3 stars vs. High Rating ≥ 4 stars), we would choose **F₁‑score** as our primary metric—because ratings are heavily skewed toward the high end, F₁ balances precision and recall on the minority class.
