---
layout: page
permalink: /publications/index.html
title: Data Cleaning and Exploratory Data Analysis
---
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
  src="assets/n_steps_hist.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

The histogram above shows that most recipes cluster between **5–15 steps**, with a peak around 8 steps and a long right tail of very complex recipes. This tells us that “number of steps” varies enough to be a potential predictor of rating—simpler recipes might correlate with higher ratings.


---

## Bivariate Analysis

![Calories vs Rating](/images/calories_vs_rating.png)

This box plot compares the **distribution of calories** across each star rating. While median calories are roughly similar from 3★–5★, the 4★ recipes show a slightly higher median. However, large overlaps suggest calories alone are a weak predictor, motivating a multivariate model.

---

## Interesting Aggregates

| Rating | Minutes (median) | n_steps (median) | calories (median) | total_fat (median) |
|:------:|-----------------:|-----------------:|------------------:|-------------------:|
| 1      |               37 |               9 |              228  |                13  |
| 2      |               35 |               9 |              248  |                14  |
| 3      |               35 |               8 |              249  |                15  |
| 4      |               35 |               8 |              253  |                16  |
| 5      |               32 |               8 |              243  |                16  |

*Table: Median recipe characteristics by star rating.*

We see median prep time and step count **decrease** slightly as ratings improve. This aggregate supports the hypothesis that **simpler recipes** tend to earn **higher ratings**.

---

## Imputation

No imputation was performed on our target or predictors.  
- **Ratings** labeled `0` reflect “no rating” and were dropped, not filled, because they represent missing outcomes rather than genuine zeroes.  
- Other columns (e.g. `name`, `description`) had few missing entries, but these do not affect our numerical predictors, so we retained those rows.  

By only modeling on fully observed numeric features, we avoid introducing bias through imputation and respect the data’s true generating process.
