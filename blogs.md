---
layout: page
permalink: /blogs/index.html
title: Final Model
---

## Final Model & Improvements
By experimenting with different prediction models, adjusting preprocessing methods, and tuning hyperparameters, we established a **Logistic Regression classifier** as our final model.


### 1 · Feature Engineered  
 - **Quantile-transformed feature**: 'minutes', 'calories', 'sugar', 'sodium', since we found their distributions are left-skewed with many large outliers at the exploration state.
 - **Standardized-transformed feature**: 'minutes', 'calories', 'sugar', 'sodium', all numeric features are standardized to make sure they are weighted in the same scale.
 - **Binary Encoding**: 'is_healthy' is encoded into 0/1 for the model to weight.


### 2 · Algorithm & Hyper-parameters  
Since different C represents different level of tolerence of model's complexity, and the optimal max_iter makes sure the model can converge, Adjusting these two parameters can make the logistic regression model achieve better results under different data characteristics and scales.

| Hyper-parameter               | Search grid                      | **Best** |
|-------------------------------|----------------------------------|---------:|
| `logisticregression__C`       |  0.001 · 0.01 · 0.1 · 1 · 10      | **0.001** |
| `logisticregression__max_iter`|  10 · 25 · 50 · 100               | **10** |

*GridSearchCV (10-fold, F₁ scoring) selected a heavily regularised model (`C = 0.001`).*  

### 3 · Test-set Performance  

| Metric              | Baseline Tree | **Final Logistic** |
|---------------------|--------------:|-------------------:|
| **F₁-score**        | 0.836         | **0.917** |


**Key change:** the logistic model achieved an F1 score of **0.917**, improving by roughly **8%** compared to the baseline Decision Tree model.


### 5 · Why the Extra Features & Transforms Helped  

- **Feature Engineering** These transformations improve model performance by addressing distribution issues (e.g., skewness and outliers), ensuring numerical features are on the same scale for regularized models, and allowing binary features to be interpreted numerically by classifiers.  
- **Regularisation** (`C = 0.001`) prevented weight explosion and over-fitting on rare “Not-High” cases.  
- Together these let the model capture the simple truth of the data-generating process: *most* Food.com recipes earn ≥ 4.5 once they pass the >3-ratings filter.

---


### 6 · Takeaways  
- With such high F1 score, we believe complexity and nutritional health information could effectively predict whether a recipe would get high or low overall ratings.
- In terms of building models, better preprocessing with transformers and regularization improved our model greatly.
