---
layout: page
permalink: /blogs/index.html
title: Final Model
---

## Step 5: Final Model & Improvements

### 1 · Feature Engineering  
Our **baseline** used raw numeric values plus a Boolean health tag. For the final round we kept the same core columns (no “future” data leakage) but **engineered their representation** to align better with our chosen algorithms:

| Column            | Why it matters | Engineering change |
|-------------------|----------------|--------------------|
| `minutes`         | Captures recipe complexity | **Quantile → Normal** transform flattens heavy‐tailed prep-time distribution (many extreme outliers) so linear models see a near-Gaussian feature. |
| `calories`, `sugar`, `sodium` | Central to nutritional “health vs. indulgence” trade-offs that we observed in EDA | Same quantile normalization to mitigate huge spikes (e.g., sugar bombs). |
| `is_healthy`      | Encodes author-declared healthfulness signal | Cast from Boolean to 0/1 so weights can be learned. |

> **Why these help**  
> - Logistic regression assumes approximately linear separability; mapping skewed variables onto a normal scale helps that assumption hold.  
> - Scaling all numeric predictors (via **StandardScaler** after the quantile step) puts them on comparable ranges, which stabilizes gradient descent and the interpretation of coefficients.  
> - Encoding `is_healthy` numerically lets the model learn whether health tags systematically raise or lower ratings.

### 2 · Algorithm Choice  
We evaluated three algorithms:

| Model | Key Traits | CV F₁ (mean) |
|-------|-----------|--------------|
| **Decision Tree (balanced)** | Non-parametric, handles non-linear splits | 0.846 |
| k-Nearest Neighbors | Distance-based, requires heavy scaling | 0.915 |
| **Logistic Regression** *(chosen)* | Interpretable linear boundary, robust with proper scaling | **0.915** |

Although k-NN tied Logistic in cross-validated F₁, Logistic Regression is **simpler, faster, and more interpretable**, so we adopted it as the Final Model.

### 3 · Hyperparameter Tuning  
We wrapped preprocessing and classifier in a single `sklearn.Pipeline` and searched with **GridSearchCV (10-fold, F₁ scoring)**:

| Hyperparameter                 | Search Grid                 | Best Value |
|--------------------------------|-----------------------------|------------|
| `logisticregression__C`        | 0.001 · 0.01 · 0.1 · 1 · 10 | **0.001** |
| `logisticregression__max_iter` | 10 · 25 · 50 · 100         | **10**    |

*(Regularization strength `C=0.001` suggests the feature space is nearly linearly separable once transformed, and heavier regularization reduces over-fitting.)*

### 4 · Performance Comparison  

| Metric (test set) | Baseline Tree | Final Logistic |
|-------------------|---------------|----------------|
| **F₁-score**      | 0.836         | **0.915** |
| Precision         | 0.807         | 0.911 |
| Recall            | 0.867         | 0.919 |
| Accuracy          | 0.734         | 0.824 |

**Relative F₁ improvement:** **+9 percentage points** (≈ +9.5 %).  
The gain comes from both higher precision (fewer average recipes flagged as hits) and higher recall (more genuine hits found).

### 5 · Error Profile (confusion-matrix view)  

|                     | **Predicted High** | **Predicted Not-High** |
|---------------------|-------------------:|-----------------------:|
| **Actual High**     | TP = 809           | FN = 91                |
| **Actual Not-High** | FP = 138           | TN = 2 076             |

*TP = true positives, FP = false positives, FN = false negatives, TN = true negatives.*



### 6 · Takeaways  
- **Better preprocessing**—specifically quantile→normal mapping plus standardization—allowed a simple linear model to outperform more complex trees.  
- **Regularization** prevented over-fitting despite class imbalance.  
- The final logistic model is concise, easy to deploy, and competitive with heavier learners.
