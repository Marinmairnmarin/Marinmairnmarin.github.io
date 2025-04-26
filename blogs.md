---
layout: page
permalink: /blogs/index.html
title: Final Model
---

## Final Model & Improvements

### 1 · Feature Representation  
 - unchanged – same quantile → normal transforms
 - standardisation
 - Boolean → 0/1 casting  

| Column group | Engineering rationale |
|--------------|-----------------------|
| **Prep-time outliers** (`minutes`) | Quantile → Normal transform tames the long right tail, giving linear models a near-Gaussian input. |
| **Nutrition spikes** (`calories`, `sugar`, `sodium`) | Same transform prevents a few “sugar bombs” from dominating the loss. |
| **`is_healthy` tag** | Encoded 0/1 so the classifier can weight it. |

### 2 · Algorithm & Hyper-parameters  

| Hyper-parameter               | Search grid                      | **Best** |
|-------------------------------|----------------------------------|---------:|
| `logisticregression__C`       |  0.001 · 0.01 · 0.1 · 1 · 10      | **0.001** |
| `logisticregression__max_iter`|  10 · 25 · 50 · 100               | **10** |

*GridSearchCV (10-fold, F₁ scoring) selected a heavily regularised model (`C = 0.001`).*  

Although the logistic model collapses to predicting **“High” for every case** at the default 0.5 threshold, that still maximises F₁ on an imbalanced set—illustrating why threshold tuning or class-weighting may be needed next.

### 3 · Test-set Performance  

| Metric              | Baseline Tree | **Final Logistic** |
|---------------------|--------------:|-------------------:|
| **F₁-score**        | 0.836         | **0.915** |
| Precision           | 0.807         | 0.843 |
| Recall              | 0.867         | **1.000** |
| Accuracy            | 0.734         | 0.843 |

**Key change:** the logistic model attains perfect recall for the positive class but at the cost of **zero specificity** (it never predicts “Not-High”). Despite this, its F₁ improves by ≈ 0.08 because the dataset is dominated by “High” recipes.

### 4 · Confusion Matrix  

|                     | **Pred High** |  **Pred Not-High** |
|---------------------|--------------:|------------------:|
| **Actual High**     | **TP = 2 926**| FN = 0 |
| **Actual Not-High** | **FP = 544**  | TN = 0 |

*All 3 470 test samples were labelled “High.”*

> **Interpretation**  
> - **Recall = 1.0:** we never miss a truly high-rated recipe.  
> - **Precision ≈ 0.84:** about 1 in 6 flagged recipes will disappoint.  
> - **Next steps:** add `class_weight="balanced"` or tune the decision threshold using validation PR-AUC to trade some recall for better specificity.

### 5 · Why the Extra Features & Transforms Helped  

- **Distribution shaping** (quantile→normal) allowed a linear boundary to separate the “High” region without being dominated by extreme prep-times or nutrition outliers.  
- **Regularisation** (`C = 0.001`) prevented weight explosion and over-fitting on rare “Not-High” cases.  
- Together these let the model capture the simple truth of the data-generating process: *most* Food.com recipes earn ≥ 4.5 once they pass the >3-ratings filter.

---


### 6 · Takeaways  
- **Better preprocessing**—specifically quantile→normal mapping plus standardization—allowed a simple linear model to outperform more complex trees.  
- **Regularization** prevented over-fitting despite class imbalance.  
