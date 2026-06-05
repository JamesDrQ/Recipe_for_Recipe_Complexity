
---

## Introduction

This project uses two datasets containing information about recipes and user interactions with those recipes. The `recipes` dataset contains recipe-level information, including each recipe's name, preparation time, number of ingredients, number of steps, nutrition information, tags, and description. The `interactions` dataset contains individual user ratings and reviews.

The two datasets were combined using `id` from the `recipes` dataset and `recipe_id` from the `interactions` dataset. After calculating the average user rating for each recipe and merging it into the recipe data, the dataset used for this analysis contains **83782 rows**, with each row representing one recipe.

The central question of this project is:

> **Is recipe complexity associated with a recipe's average user rating?**

In this project, recipe complexity is measured using the preparation time, number of ingredients, and number of preparation steps. This question is worth studying because complexity may affect how users experience a recipe. More complex recipes may be difficult or time-consuming to follow, potentially resulting in lower ratings. However, complex recipes may also produce more impressive results and receive higher ratings. Understanding this relationship could help recipe creators design recipes that balance quality with convenience and could help cooking platforms recommend recipes that better match users' preferences and available time.

The columns most relevant to this question are:

| Column          | Description                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------ |
| `id`            | The unique identifier assigned to each recipe.                                                               |
| `minutes`       | The total number of minutes required to prepare the recipe.                                                  |
| `n_ingredients` | The number of ingredients used in the recipe.                                                                |
| `n_steps`       | The number of preparation steps in the recipe instructions.                                                  |
| `avg_rating`    | The average of all valid user ratings submitted for the recipe.                                              |
| `tags`          | Labels describing characteristics of the recipe, such as its difficulty, preparation time, or meal category. |

The primary complexity variables are `minutes`, `n_ingredients`, and `n_steps`, while `avg_rating` is the outcome used to evaluate whether recipe complexity is associated with user ratings.


## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The `recipes` dataset contains one row per recipe, while the `interactions` dataset contains one row per user interaction. Since multiple users may rate the same recipe, recipes can appear many times in `interactions`.

Ratings equal to 0 were replaced with missing values because valid ratings range from 1 to 5, and 0 represents a user who did not provide a rating. Keeping these values would incorrectly lower recipe averages.
Next, I calculated the average valid rating for each recipe and merged it into the `recipes` dataset using the recipe ID. A left merge was used to preserve all recipes, including those without valid ratings. Recipes with no valid ratings have a missing `avg_rating` rather than a misleading value of 0.
After cleaning, each row represents one recipe, allowing recipe features such as preparation time, number of ingredients, and number of steps to be compared with its average user rating.

#### Head of the Cleaned DataFrame

| name                                 |   minutes |   n_steps |   n_ingredients |   avg_rating |
|:-------------------------------------|----------:|----------:|----------------:|-------------:|
| 1 brownies in the world    best ever |        40 |        10 |               9 |            4 |
| 1 in canada chocolate chip cookies   |        45 |        12 |              11 |            5 |
| 412 broccoli casserole               |        40 |         6 |               9 |            5 |
| millionaire pound cake               |       120 |         7 |               7 |            5 |
| 2000 meatloaf                        |        90 |        17 |              13 |            5 |

### Univariate Analysis

<iframe src="{{ site.baseurl }}/assets/n_ingredients_distribution.html" width="100%" height="450" frameborder="0"></iframe>

The distribution of the number of ingredients is unimodal and right-skewed. The center of the distribution appears to be around 8 ingredients. This suggests that most recipes use a moderate number of ingredients, and recipes with ingredient counts of more than 25 are relatively uncommon.

### Bivariate Analysis

<iframe src="{{ site.baseurl }}/assets/avg_rating_by_n_ingredients.html" width="800%" height="450" frameborder="0"></iframe>

This box plot shows the distribution of `avg_rating` across different ingredient-count groups. The average ratings are concentrated near 5, and the medians all equal to 5. The boxes are also similar across the four groups, suggesting that recipes with more ingredients do not clearly receive higher or lower ratings than recipes with fewer ingredients. Although there are some low-rating outliers in each group, the overall pattern does not show a strong association between the number of ingredients and average rating.

### Interesting Aggregates

| ingredient_group / step_group |   1-5   |  6-10   |  11-20  |   21+   |
|:------------------------------|--------:|--------:|--------:|--------:|
| 1-5                           | 4.66479 | 4.63466 | 4.62094 | 4.6594  |
| 6-10                          | 4.62164 | 4.61032 | 4.61711 | 4.6623  |
| 11-15                         | 4.61855 | 4.61532 | 4.62361 | 4.65194 |
| 16+                           | 4.70382 | 4.63493 | 4.65392 | 4.62091 |

This pivot table shows the mean `avg_rating` for recipes grouped by both number of ingredients and number of steps. The average ratings are very similar across most combinations, mostly ranging from about 4.61 to 4.70. This suggests that even when considering both forms of recipe complexity together, recipes with more ingredients or more steps do not clearly receive higher or lower ratings. Overall, the table supports the earlier box plots: recipe complexity does not appear to have a strong association with average rating.

## Assessment of Missingness

Missingness analysis goes here.

## Hypothesis Testing

Hypothesis testing content goes here.

## Framing a Prediction Problem

Prediction problem content goes here.

## Baseline Model

Baseline model content goes here.

## Final Model

Final model content goes here.

## Fairness Analysis

Fairness analysis content goes here.
