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

## News and Updates

- **June 2024**：Very excited to be selected as [KDD UC Scholar](https://kdd2024.kdd.org/undergraduate-consortium/). See you in Spain!
- **May 2024**：My bachelor thesis won the Annual Best Thesis Award (Top 1/300).
- **April 2024**：Our work *BLEGuard* has been accepted to [MobiSys 2024](https://www.sigmobile.org/mobisys/2024/) as a poster paper. See you in Japan!
- **March 2024**：Very excited to get a MPhil offer from Engineering department at Cambridge University!
- **Dec 2023**：Very excited to be selected as [AAAI UC Scholar](https://aaai.org/aaai-conference/undergraduate-consortium-program/). See you in Canada!
- **Jun 2022**：Started research programme at [Cambridge AI Group](https://www.cl.cam.ac.uk/research/ai/), advised by Prof. Pietro Liò.

<br>

<!-- <blockquote class="twitter-tweet"><p lang="en" dir="ltr">Thrilled to be an AAAI-UC Scholar at <a href="https://twitter.com/hashtag/AAAI24?src=hash&amp;ref_src=twsrc%5Etfw">#AAAI24</a>, thanks to <a href="https://twitter.com/hashtag/AAAI?src=hash&amp;ref_src=twsrc%5Etfw">#AAAI</a> &amp; <a href="https://twitter.com/hashtag/GoogleExploreCSR?src=hash&amp;ref_src=twsrc%5Etfw">#GoogleExploreCSR</a> for the sponsorship. Grateful for the knowledge gained and new friendships formed.<br><br>Wonderful trip in Vancouver. Looking forward to staying connected with all.<a href="https://twitter.com/hashtag/AAAI24?src=hash&amp;ref_src=twsrc%5Etfw">#AAAI24</a> <a href="https://twitter.com/hashtag/Vancouver?src=hash&amp;ref_src=twsrc%5Etfw">#Vancouver</a> <a href="https://twitter.com/hashtag/GoogleExploreCSR?src=hash&amp;ref_src=twsrc%5Etfw">#GoogleExploreCSR</a> <a href="https://t.co/wUQUp8XlSM">pic.twitter.com/wUQUp8XlSM</a></p>&mdash; Hanlin CAI (seeking a PhD position 2025) (@lancecai2002) <a href="https://twitter.com/lancecai2002/status/1762210025173344260?ref_src=twsrc%5Etfw">February 26, 2024</a></blockquote> 
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> -->
