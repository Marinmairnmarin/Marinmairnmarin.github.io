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
| **1. Drop Nan** | rating-related nan rows are dropped (reason discussed above). |
| **2. Filter to recipes with > 3 ratings** | 'avg_rating' is used as as a summary of each recipe’s overall rating. Averages based on 1-2 votes are too noisy to trust. |
| **3. Extract Interested Columns** | 'avg_rating' represents recipes' overally ratings, 'minutes', 'n_steps',  and 'n_ingredients' represent complexity,'nutrition' and 'tags' bring nutritional health information. They contain all the information we are interested to investigate.
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

**Interpretation**
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


**Interpretation**
The ratings distribution for low-calorie and medium-calorie recipes isrelatively balanced, as they appear across the entire rating spectrum. However, we observed a modest uptick in average calories at the highest rating bins. This might suggests that there is a segment of users who reward richer, higher-calorie dishes with top scores—justifying calories as a useful feature in our prediction model.


---

## Interesting Aggregates

### Mean & Median of Key Metrics by Rating Bin

**Mean**

| rating_bin | minutes | n_steps | n_ingredients | calories | sugar | sodium | saturated_fat |
|------------|---------|---------|----------------|----------|-------|--------|----------------|
| 1          | 19.75   | 7.25    | 6.00           | 237.12   | 65.00 | 8.00   | 17.00          |
| 2          | 79.18   | 8.65    | 8.47           | 255.66   | 37.47 | 18.53  | 23.15          |
| 3          | 82.46   | 9.99    | 9.37           | 332.47   | 31.78 | 22.05  | 29.28          |
| 4          | 72.23   | 9.34    | 8.95           | 313.39   | 29.87 | 21.27  | 27.38          |
| 5          | 60.64   | 9.56    | 8.81           | 303.72   | 32.86 | 20.05  | 27.78          |


<iframe src="{{ '/assets/mean_by_rating.html' | relative_url }}" width="100%" height="300" frameborder="0"></iframe>
<iframe src="{{ '/assets/median_by_rating.html' | relative_url }}" width="100%" height="300" frameborder="0"></iframe>

**Significance:**
- The mean and median distribution indicate there are **many large outliers** in `minutes`, `calories`, `sugar`, `sodium`, and `saturated_fat`, which suggests that we should probably apply some feature engineering on them if we want to use for prediction.

- Pattern: it seems low-sugar recipes tend to have higher ratings.  The complexity, calories, sodium seem to follow a bell-shaped distribution with ratings. Perhaps people acknowledge the taste of food made from more complex/unhealthy recipes, but slightly lower their ratings due to the complexity/unhealthiness, which results in such a distribution.

---