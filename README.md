
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

Data cleaning and EDA content goes here.

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
