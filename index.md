---
layout: page
---

# About Us

Here are **Zhicong and Tianxin**.<br>

---

## Recipes and Ratings

This project uses the **Recipes and Ratings** dataset from Food.com, originally scraped for a recommender‑systems study. We focus on how recipe characteristics (prep time, nutritional content, complexity) relate to average user ratings.

### Dataset Overview

- **Recipes file** (`RAW_recipes.csv`)  
  - **Rows:** 16,000  
  - **Columns:**  
    | Column           | Description                                                                                       |
    |------------------|---------------------------------------------------------------------------------------------------|
    | `name`           | Recipe name                                                                                       |
    | `id`             | Unique recipe identifier                                                                          |
    | `minutes`        | Time (in minutes) required to prepare the recipe                                                  |
    | `contributor_id` | ID of the user who submitted the recipe                                                           |
    | `submitted`      | Date the recipe was submitted (YYYY-MM-DD)                                                        |
    | `tags`           | List of Food.com tags (e.g., “dessert,” “quick”)                                                  |
    | `nutrition`      | String-encoded list: `[calories, total fat (%DV), sugar (%DV), sodium (%DV), protein (%DV), saturated fat (%DV), carbohydrates (%DV)]` |
    | `n_steps`        | Number of steps in the recipe                                                                     |
    | `steps`          | Text instructions, in order                                                                       |
    | `description`    | User-provided description or notes                                                                |

- **Ratings file** (`RAW_interactions.csv`)  
  - **Rows:** 200,000  
  - **Columns:**  
    | Column      | Description                                |
    |-------------|--------------------------------------------|
    | `user_id`   | ID of the user who left the review         |
    | `recipe_id` | Recipe ID (links to `id` in recipes file)  |
    | `date`      | Date of interaction (YYYY-MM-DD)           |
    | `rating`    | Rating score (1–5; 0 indicates “no rating”) |
    | `review`    | Free-text review                           |

- **Merged dataset** (left-merge of recipes + ratings)  
  - **Rows:** ~210,000  
  - **Key additional column:**  
    | Column       | Description                                                    |
    |--------------|----------------------------------------------------------------|
    | `avg_rating` | Mean of non-zero ratings per recipe (we set 0→NaN before averaging) |

### Core Question

> **How do recipe features—prep time, complexity, and nutrition—relate to average user ratings?**

We’ll examine:
1. **Prep time vs. average rating:** Do quicker recipes score higher?
2. **Nutrition vs. average rating:** Which nutrients (e.g., protein, sugar) predict popularity?
3. **Complexity vs. average rating:** Does more detail (more steps) improve ratings?

### Why Readers Should Care

- **Home cooks** can pick recipes that are both quick and highly rated.  
- **Health-conscious readers** learn which nutritious recipes users love most.  
- **Food bloggers/developers** gain insights to craft content that resonates (e.g., high-protein, low-prep recipes).

---
