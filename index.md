---
layout: page
---

# About Us

Here are **Zhicong and Tianxin**.<br>

---

## Recipes and Ratings

This project uses the **Recipes and Ratings** dataset from Food.com, originally scraped for a recommender‑systems study. We focus on how recipe characteristics (prep time, nutritional content, complexity) relate to average user ratings.

## Introduction
We’re working with the Food.com Recipes & Ratings dataset, which combines two tables of user‑submitted recipes and their corresponding reviews since 2008. Our project centers on a single, focused question:

> **Can we predict a recipe’s average user rating using only its culinary complexity (prep time, number of steps, number of ingredients) and its nutritional profile (calories, fat, sugar, sodium, protein, saturated fat, carbohydrates)?**

Home cooks, nutritionists, and recipe‑platform designers should care about this question because accurate rating predictions can:
- **Help users discover** recipes they’re more likely to enjoy  
- **Support healthier choices** by revealing how nutrition influences ratings  
- **Optimize recommendation engines** for both taste and health  

---

## Dataset Size
- **Number of unique recipes (rows)**: 23,820  
- **Number of columns in the merged dataset**: 14  

*(Note: after merging recipes and interactions, filling zero‑ratings as NaN, and computing each recipe’s average rating, we distilled our modeling table to one row per recipe.)*

---

## Relevant Columns & Descriptions

| Column            | Description                                                                                 |
|-------------------|---------------------------------------------------------------------------------------------|
| **minutes**       | Preparation time in minutes                                                                 |
| **n_steps**       | Number of instruction steps in the recipe                                                  |
| **n_ingredients** | Total count of distinct ingredients used                                                    |
| **calories**      | Total calories per serving                                                                  |
| **total_fat**     | Percent daily‑value (PDV) of total fat per serving                                          |
| **sugar**         | PDV of sugar per serving                                                                    |
| **sodium**        | PDV of sodium per serving                                                                   |
| **protein**       | PDV of protein per serving                                                                   |
| **saturated_fat** | PDV of saturated fat per serving                                                            |
| **carbohydrates** | PDV of total carbohydrates per serving                                                      |
| **avg_rating**    | The recipe’s mean user rating (1–5), computed by averaging all nonzero ratings submitted    |

These are the features our model will draw upon when learning to predict **avg_rating**. By understanding how preparation complexity and nutritional makeup jointly drive popularity, we aim to build a recommender that balances taste‑appeal with health considerations.
