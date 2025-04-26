---
layout: page
---
# High or Low? Predict Recipes Rating with Complexity & Nutritional Health Information

## Introduction

This project uses the **Recipes and Ratings** dataset from **[Food.com](https://www.food.com/)**, which combines two tables of user‑submitted recipes and their corresponding reviews originally scraped for a recommender‑systems study. And We are interested in exploring:
>  **How do a recipe's nutritional health information and complexity relate to its ratings?**

## Why it matters
Home cooks, nutritionists, and recipe‑platform designers should care because understanding these relationships can:
- **Help users discover** recipes they’re more likely to enjoy  
- **Support healthier choices** by revealing how nutrition influences ratings  
- **Optimize recommendation engines** for both taste and health  

---
## Preparing the data
- **Merging**We left merged the datasets to get all reviews of recipes included in the recipes dataset.**NaN**
- **Zero-ratings** Change to **NaN**
In reality, the lowest score a reviewer can actively give is 1. A recorded value of 0 almost always means no rating was actually provided even though the review text was submitted. We therefore convert rating = 0 to NaN before analysis so that missing opinions don’t bias averages.
- **Averaging Rating** We calculated the average rating per recipe and added it as a new column.**NaN**

## Dataset Size
- **Number of unique recipes (rows)**: 234429  
- **Number of columns in the merged dataset**: 17


---

## Relevant Columns & Descriptions

| Column            | Description                                                                                 |
|-------------------|---------------------------------------------------------------------------------------------|
| **minutes**       | Preparation time in minutes                                                                 |
| **n_steps**       | Number of instruction steps in the recipe                                                   |
| **n_ingredients** | Total count of distinct ingredients used                                                    |
| **tags**          | Food.com tags for recipe (which contains some health-related tags)                          |
| **nutrition**     | List [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV = % daily value                                                                  |
| **avg_rating**    | Mean user rating (1–5), computed by averaging all nonzero ratings submitted                 |

These are the features we are interested to investigate. By understanding how preparation complexity and nutritional makeup jointly drive popularity, we aim to build a recommender that balances taste‑appeal with health considerations.
