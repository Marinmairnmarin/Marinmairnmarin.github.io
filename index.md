---
layout: page
---

## Recipes and Ratings

This project uses the **Recipes and Ratings** dataset from Food.com, originally scraped for a recommender‑systems study. We focus on how recipe characteristics (prep time, nutritional content, complexity) relate to average user ratings.

## Introduction
We’re working with the Food.com Recipes & Ratings dataset, which combines two tables of user‑submitted recipes and their corresponding reviews since 2008. Our project centers on a single, focused question:

> **How do a recipe's nutritional health information and complexity relate to its ratings?**

## Why it matters
Home cooks, nutritionists, and recipe‑platform designers should care because understanding these relationships can:
- **Help users discover** recipes they’re more likely to enjoy  
- **Support healthier choices** by revealing how nutrition influences ratings  
- **Optimize recommendation engines** for both taste and health  

---
## Preparing the data
- **Zero-ratings** Change to **NaN**
In reality, the lowest score a reviewer can actively give is 1. A recorded value of 0 almost always means no rating was actually provided even though the review text was submitted. We therefore convert rating = 0 to NaN before analysis so that missing opinions don’t bias averages.

After merging the two tables, filling zero‑ratings as NaN, and computing each recipe’s avg_rating, we reduce the dataset to one row per recipe.

## Dataset Size
- **Number of unique recipes (rows)**: 23,820  
- **Number of columns in the merged dataset**: 14  


---

## Relevant Columns & Descriptions

| Column            | Description                                                                                 |
|-------------------|---------------------------------------------------------------------------------------------|
| **minutes**       | Preparation time in minutes                                                                 |
| **n_steps**       | Number of instruction steps in the recipe                                                   |
| **n_ingredients** | Total count of distinct ingredients used                                                    |
| **tags**          | Food.com tags for recipe                                                                    |
| **nutrition**     | List [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV = % daily value                                         |
| **avg_rating**    | Mean user rating (1–5), computed by averaging all nonzero ratings submitted                 |


These are the features our model will draw upon when learning to predict **avg_rating**. By understanding how preparation complexity and nutritional makeup jointly drive popularity, we aim to build a recommender that balances taste‑appeal with health considerations.
