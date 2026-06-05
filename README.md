
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

The recipes dataset contains one row per recipe, while the interactions dataset contains one row per user interaction. Since multiple users may rate the same recipe, recipes can appear many times in interactions.
Ratings equal to 0 were replaced with missing values because valid ratings range from 1 to 5, and 0 represents a user who did not provide a rating. Keeping these values would incorrectly lower recipe averages.
Next, I calculated the average valid rating for each recipe and merged it into the recipes dataset using the recipe ID. A left merge was used to preserve all recipes, including those without valid ratings. Recipes with no valid ratings have a missing avg_rating rather than a misleading value of 0.
After cleaning, each row represents one recipe, allowing recipe features such as preparation time, number of ingredients, and number of steps to be compared with its average user rating.

### Head of the Cleaned DataFrame

| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                            | nutrition                                     |   n_steps | steps                                                           | description                                                     | ingredients                                                     |   n_ingredients |   avg_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------------------------------------|:----------------------------------------------|----------:|:----------------------------------------------------------------|:----------------------------------------------------------------|:----------------------------------------------------------------|----------------:|-------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingre... | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]      |        10 | ['heat the oven to 350f and arrange the rack in the middle',... | these are the most; chocolatey, moist, rich, dense, fudgy, d... | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granul... |               9 |            4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparati... | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]  |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift... | this is the recipe that we use at my school cafeteria for ch... | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', ... |              11 |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingre... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]     |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish... | since there are already 411 recipes for broccoli casserole p... | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp che... |               9 |            5 |
| millionaire pound cake               | 286009 |       120 |           461724 | 2008-02-12  | ['time-to-make', 'course', 'cuisine', 'preparation', 'occasi... | [878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0] |         7 | ['freheat the oven to 300 degrees', 'grease a 10-inch tube p... | why a millionaire pound cake?  because it's super rich!  thi... | ['butter', 'sugar', 'eggs', 'all-purpose flour', 'whole milk... |               7 |            5 |
| 2000 meatloaf                        | 475785 |        90 |          2202916 | 2012-03-06  | ['time-to-make', 'course', 'main-ingredient', 'preparation',... | [267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]    |        17 | ['pan fry bacon , and set aside on a paper towel to absorb e... | ready, set, cook! special edition contest entry: a mediterra... | ['meatloaf mixture', 'unsmoked bacon', 'goat cheese', 'unsal... |              13 |            5 |

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
