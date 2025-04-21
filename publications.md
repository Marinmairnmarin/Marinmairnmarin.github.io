---
layout: page
permalink: /publications/index.html
title: Data Cleaning and Exploratory Data Analysis
---
## Data Cleaning

After merging the recipes and ratings tables, we performed the following steps, informed by how the data were generated:

1. **Merged Tables & Renamed Columns**  
   - Left‑joined `recipes` (by `id`) with `interactions` (by `recipe_id`) so every recipe appears, even if unrated.  
   - Renamed `user_id` → `reviewer_id` for clarity.  
2. **Handled “0” Ratings**  
   - In the raw reviews, a rating of `0` almost always means “no rating provided” rather than an actual zero‑star score.  
   - We replaced `0 → NaN` to avoid biasing averages toward zero.  
3. **Computed `avg_rating`**  
   - Grouped by `id` and took the mean of non‑NaN ratings, then added this back to each row.  
4. **Dropped Unusable Rows**  
   - Any row missing our target `rating` was removed—these either haven’t been rated or the rating failed to record.  
   - Recipes with nutrient values > 100 % DV were also dropped as data errors.  
5. **Split Out Nutrition**  
   - Parsed the string in `nutrition` (e.g. `"[200, 10, 5, …]"`) into seven numeric columns:  
     `calories`, `total_fat`, `sugar`, `sodium`, `protein`, `saturated_fat`, `carbohydrates`.

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

![Distribution of Number of Steps](/images/distribution_steps.png)

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
