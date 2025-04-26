---
layout: page
permalink: /publications/index.html
title: Data Cleaning and Exploratory Data Analysis
---

## Imputation
No imputation was performed.

We found nan value in 2 categories:

**1. name, description, date, review**: Their absence does not prevent us from obtaining nutritional health information and complexity from other columns, so we decided to keep the data records with these Nan columns.

**2. reviewer_id, rating, avg_rating**: reviewer_id and avg_rating will only be Nan when a recipe does not match any reviewer's reviews, and the Nan of rating may come from the absence of reviews or the existence of reviews but the value of the rating itself is not successfully entered. Since exploring how recipes are rated are what we want to do next, we drop them at this stage.

## Data Cleaning

## 1  Data Cleaning

| Step | Why |
|:--|:--|:--|
| **1. Drop Nan** | rating-related nan rows are dropped (reason discussion above). |
| **2. Filter to recipes with > 3 ratings** | 'avg_rating' is used as as a summary of each recipe’s overall rating. Averages based on 1-2 votes are too noisy to trust. |
| **3. Extract Interested Columns** | 'avg_rating' represents recipes' overally ratings, 'minutes', 'n_steps',  and 'n_ingredients' represent complexity,'nutrition' and 'tags' bring nutritional health information.
| **4. Extract nutrition components from `nutrition`** | 'nutrition' column is in the form of a string to mimic a list. We toke the nutition components out and converted to numeric data for health analysis ('nutrition' is dropped after extraction). |
| **5. Remove implausible nutrition rows** | Values far past 100 %DV are likely scraped/entry errors (e.g., “30 000 % sugar”), so only values ≤ 150 are kept. |
| **6. Add `is_healthy` flag from `tags`** | We marked `tags` containing *healthy*, *healthy-2*, *high-in-something-diabetic-friendly* true for a new column `is_healthy`, since tags are author-entered meta-data whcih summaize the overall healthiness better than single nutrients ('tags' is dropped after extraction). |

(Note: Although there are tags like low-saturated-fat, they only reflect one aspect of nutrition. Since we cannot guarantee the overall healthiness of the recipe based on such tags alone, we only selected tags that more clearly indicate a healthy recipe as a whole.)


After the data cleaning, there are 13878 rows left.

Below is the head of the cleaned modeling table (one row per rated recipe):

| id     | avg_rating | minutes | n_steps | n_ingredients | calories | total_fat | sugar | sodium | protein | saturated_fat | carbohydrates | is_healthy |
|--------|------------|---------|---------|----------------|----------|-----------|-------|--------|---------|---------------|---------------|------------|
| 275030 | 5.00       | 45      | 11      | 9              | 577.7    | 53.0      | 149.0 | 19.0   | 14.0    | 67.0          | 21.0          | False      |
| 275061 | 4.78       | 65      | 16      | 11             | 402.5    | 37.0      | 9.0   | 40.0   | 42.0    | 56.0          | 8.0           | False      |
| 275071 | 4.86       | 90      | 7       | 9              | 166.1    | 10.0      | 6.0   | 0.0    | 7.0     | 4.0           | 7.0           | True       |
| 275072 | 4.25       | 35      | 8       | 11             | 227.5    | 12.0      | 6.0   | 11.0   | 67.0    | 12.0          | 1.0           | False      |
| 275094 | 4.90       | 12      | 3       | 5              | 99.5     | 7.0       | 23.0  | 14.0   | 11.0    | 3.0           | 3.0           | True       |


---

## Univariate Analysis

<iframe
  src="{{ '/assets/n_steps_hist.html' | relative_url }}"
  width="100%"
  height="600"
  frameborder="0">
</iframe>
The histogram above shows that most recipes cluster between **5–15 steps**, with a peak around 8 steps and a long right tail of very complex recipes. This tells us that “number of steps” varies enough to be a potential predictor of rating—simpler recipes might correlate with higher ratings.


---

## Bivariate Analysis

### Calories vs Average Rating

<iframe
  src="{{ '/assets/calories_vs_rating.html' | relative_url }}"
  width="100%"
  height="600"
  frameborder="0">
</iframe>


*Interpretation.*  
Although calories are fairly evenly spread across mid-range ratings, we observe a modest uptick in average calories at the highest rating bins. This suggests that while some home cooks prefer lighter recipes, there is a segment of users who reward richer, higher-calorie dishes with top scores—justifying calories as a useful feature in our prediction model.


---

## Interesting Aggregates

### Mean & Median of Key Metrics by Rating Bin

<iframe src="{{ '/assets/mean_by_rating.html' | relative_url }}" width="100%" height="300" frameborder="0"></iframe>
<iframe src="{{ '/assets/median_by_rating.html' | relative_url }}" width="100%" height="300" frameborder="0"></iframe>

*Significance of Outliers & Next Steps:*  
You’ll notice **many large outliers** in `minutes`, `calories`, `sugar`, `sodium`, and `saturated_fat`. These extreme values—e.g. recipes claiming thousands of percent daily value—signal entry or scraping errors. To handle them before modeling, we apply transformers like **QuantileTransformer** to compress tails and ensure models aren’t dominated by a few extreme recipes. This outlier-driven decision also guides our feature‐selection: we keep metrics that show reliable variation without excessive noise.


---