---
layout: page
permalink: /publications/index.html
title: Data Cleaning and Exploratory Data Analysis
---

## Imputation
We found the nan value in 2 categories:
**1. name, description, date, review**: Their absence does not prevent us from obtaining nutritional health information and complexity from other columns, so we decided to keep the data records with these Nan columns.
**2. reviewer_id, rating, avg_rating**: reviewer_id and avg_rating will only be Nan when a recipe does not match any reviewer's reviews, and the Nan of rating may come from the absence of reviews or the existence of reviews but the value of the rating itself is not successfully entered. Since exploring how recipes are rated are what we want to do next, we drop them at this stage.

## Data Cleaning

## 1  Data Cleaning (*tied to the data-generating process*)

| Step | What we did | Why (link to raw-data quirks) |
|:--|:--|:--|
| **1. Merge tables** | `recipes` ⊲⊳ `ratings` on id (left-join) | Every posted recipe should appear even if no-one has rated it yet. |
| **2. Rename columns** | `user_id → reviewer_id` | Clarifies that the ID belongs to the reviewer, not the recipe author. |
| **3. Treat “0” ratings as missing** | `0 → NaN` | In Food.com’s UI a blank star is stored as 0, but it is **not** a real zero-star review. Keeping it would under-state true ratings. |
| **4. Add `avg_rating`** | Mean of non-NaN ratings per recipe | Gives one outcome number per recipe for modelling. |
| **5. Drop rows with missing ratings** | `rating`, `avg_rating`, `reviewer_id` must be present | Rows with no outcome cannot train or test a predictive model. |
| **6. Filter to recipes with > 3 ratings** | `groupby(id).filter(count>3)` | Averages based on 1-2 votes are too noisy to trust. |
| **7. Expand `nutrition` string → seven numeric columns** | `['calories', 'total_fat', …]` | Each element is %DV, needed individually for health analysis. |
| **8. Remove implausible nutrition rows** | Keep rows where every %DV ≤ 150 | Values far past 100 %DV are likely scraped/entry errors (e.g., “30 000 % sugar”). |
| **9. Add `is_healthy` flag from `tags`** | `tags` containing *healthy*, *healthy-2*, *high-in-something-diabetic-friendly* → `True` | Tags are author-entered meta-data; they summarise overall healthiness better than single nutrients. |


Below is the head of the cleaned modeling table (one row per rated recipe):

| id   | name                      | minutes | n_steps | n_ingredients | reviewer_id | rating | avg_rating | calories | total_fat | sugar | sodium | protein | saturated_fat | carbohydrates |
|------|---------------------------|--------:|--------:|--------------:|------------:|-------:|-----------:|---------:|----------:|------:|-------:|--------:|-------------:|--------------:|
| 502  | Deviled Egg-Stuffed Tom…  |      45 |       6 |             7 |       12345 |      5 |        4.2 |      190 |        10 |     5 |      8 |      15 |            3 |            12 |
| 814  | Quick Black Bean Soup     |      30 |       5 |             8 |       67890 |      4 |        3.8 |      250 |        12 |    10 |     14 |      12 |            5 |            35 |
| 1290 | One‑Pan Lemon Chicken     |      40 |       7 |            10 |       23456 |      5 |        4.5 |      350 |        15 |     8 |     20 |      18 |            7 |            30 |
| 2075 | Easy Veggie Stir‑Fry      |      20 |       4 |             9 |       34567 |      3 |        3.1 |      180 |         8 |     6 |     12 |      14 |            2 |            25 |
| 3321 | Classic Beef Stroganoff   |      50 |       8 |            12 |       45678 |      4 |        4.0 |      420 |        18 |    12 |     22 |      20 |           10 |            40 |

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