
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

| Ingredient / Step |   1-5   |  6-10   |  11-20  |   21+   |
|:------------------|--------:|--------:|--------:|--------:|
| 1-5               | 4.66479 | 4.63466 | 4.62094 | 4.6594  |
| 6-10              | 4.62164 | 4.61032 | 4.61711 | 4.6623  |
| 11-15             | 4.61855 | 4.61532 | 4.62361 | 4.65194 |
| 16+               | 4.70382 | 4.63493 | 4.65392 | 4.62091 |

This pivot table shows the mean `avg_rating` for recipes grouped by both number of ingredients and number of steps. The average ratings are very similar across most combinations, mostly ranging from about 4.61 to 4.70. This suggests that even when considering both forms of recipe complexity together, recipes with more ingredients or more steps do not clearly receive higher or lower ratings. Overall, the table supports the earlier box plots: recipe complexity does not appear to have a strong association with average rating.

## Assessment of Missingness

### NMAR Analysis

I believe that the missingness of `description` may be **NMAR**. Whether a recipe contributor provides a description may depend on the unobserved description itself. For example, contributors may leave the description blank when they have little additional information to provide or when they believe the recipe is simple and does not require an explanation. Therefore, the probability that `description` is missing may depend on its missing content or length.

Useful information could include whether the description field was optional, each contributor's past tendency to provide descriptions, and whether contributors were prompted to complete the field. By using these additional data we might explain the missingness and potentially make it MAR.

### Missingness Dependency

<iframe src="{{ site.baseurl }}/assets/n_steps_by_missingness_of_avg_rating.html" width="800%" height="450" frameborder="0"></iframe>

Regarding the central question of whether recipe complexity is associated with a recipe's average user rating, it is important to first determine whether the missingness of `avg_rating` is related to measure of recipe complexity, such as `n_steps`. The distributions of `n_steps` for recipes with missing and non-missing `avg_rating` overlap substantially, but there are some noticeable differences. Recipes with missing `avg_rating` appear to have a slightly more spread-out distribution and a heavier right tail, suggesting that recipes with more steps may be more likely to have missing average ratings.

The permutation test produced a p-value below 0.001, let alone the significance level of 0.05. Therefore, we reject the null hypothesis that the missingness of `avg_rating` is unrelated to `n_steps`. This provides evidence that the missingness of `avg_rating` depends on the number of recipe steps.




## Hypothesis Testing

To investigate the central question of whether recipe complexity is associated with average user ratings, I examined whether recipes with more ingredients tend to receive different average ratings than recipes with fewer ingredients.

Recipes were divided into two groups using the median number of ingredients:

* **Simple recipes:** recipes with `n_ingredients` less than or equal to the median
* **Complex recipes:** recipes with `n_ingredients` greater than the median

Using the median creates two reasonably sized groups and avoids choosing an arbitrary cutoff for recipe complexity.

### Hypotheses

**Null Hypothesis:** Simple and complex recipes have the same mean `avg_rating`. Any observed difference between their mean ratings is due to random variation.

**Alternative Hypothesis:** Simple and complex recipes have different mean `avg_rating`.

### Test Statistic and Significance Level

The test statistic is the absolute difference in mean `avg_rating` between complex and simple recipes:

`|mean rating of complex recipes − mean rating of simple recipes|`

The absolute difference is appropriate because the alternative hypothesis is two-sided: I am testing whether the groups have different ratings, without assuming in advance which group has the higher rating.

I used a significance level of **0.05**, a commonly used threshold for determining whether the observed evidence is unlikely under the null hypothesis.

### Results

The observed difference in mean average rating was **0.004**. After randomly shuffling the complexity-group labels **1000** times, the permutation test produced a p-value of **0.374**.

Because the p-value is **above** the significance level of 0.05, I **fail to reject** the null hypothesis. This suggests that I do not have enough evidence to conclude that simple and complex recipes have different average ratings.

## Framing a Prediction Problem

Originally, I considered predicting a recipe’s average user rating. However, the distribution of `avg_rating` is highly skewed, with many recipes receiving ratings close to or equal to 5. As a result, a model could appear successful by predicting high ratings for nearly every recipe without learning meaningful relationships. Therefore, I chose to predict a more interpretable measure of recipe complexity.

The prediction problem is to predict the number of steps required to prepare a recipe. This is a **regression problem** because the response variable, `n_steps`, is numerical. Predicting `n_steps` is meaningful because the number of instructions is one measure of how complicated a recipe may be to follow. For example, two recipes may require similar preparation times and numbers of ingredients, but the recipe with more steps may require more effort from the cook.

At the time of prediction, the model would know information available from the recipe’s basic description and metadata, including its preparation time, number of ingredients, name, description, and tags. The model would not use the actual written steps or any features directly derived from `n_steps`, since that information would reveal the response variable.

I use **root mean squared error (RMSE)** as the primary evaluation metric. RMSE measures the typical prediction error in the same unit as the response variable: number of steps. It also penalizes large errors more heavily than mean absolute error, which is useful because substantially underestimating or overestimating the number of steps would make the prediction less useful. Unlike (R^2), RMSE directly communicates approximately how many steps the model’s predictions differ from the actual values.


## Baseline Model

For the baseline model, I used a **Linear Regression** model to predict `n_steps` using two features: `minutes` and `n_ingredients`. Both are quantitative features. These features were selected because recipes that require more time or use more ingredients may also require more instructions.

The `minutes` column is highly right-skewed and contains several extreme values, so I applied a logarithmic transformation using `log1p`. This reduces the influence of unusually large preparation times while keeping all recipes in the dataset. I left `n_ingredients` unchanged because it has a much smaller range and is less affected by extreme values.

The model was implemented using a single sklearn `Pipeline`. A `ColumnTransformer` first applied the log transformation to `minutes` and passed through `n_ingredients` without modification. The transformed features were then used to fit a Linear Regression model. Because both features are already quantitative, no categorical encoding was necessary.

This baseline model uses:

* **2 quantitative features:** `minutes` and `n_ingredients`
* **0 ordinal features**
* **0 nominal features**

On the held-out test set, the baseline model achieved an **RMSE of 5.56 steps** and an **R² score of 0.24**. The RMSE indicates that the model’s predictions differ from the actual number of steps by approximately 5.56 steps on average. The R² score indicates that the model explains about 24% of the variation in `n_steps`.

I do not consider this model especially strong because much of the variation in the number of steps remains unexplained, and an error of approximately 5.56 steps can be substantial for shorter recipes. However, it provides a reasonable and interpretable reference point for evaluating whether the final model improves prediction performance.


## Final Model

Final model content goes here.

## Fairness Analysis

Fairness analysis content goes here.
