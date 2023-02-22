<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

# Calorie Hunters :D
This is a project for the Winter 2023 offering of DSC 80 at UC San Diego, conducting EDA on recipe and review data from food.com. 

Members: Kurumi Kaneko, Candus Shi

## Introduction
For this project we were provided two datasets from [food.com](food.com). 
One was about recipes with numerical and textual data 
like minutes, number of steps/ingredients, and tags. 
The other dataset was about interactions which include ratings and reviews 
for specific recipes. Our analysis was focused on answering the question: 
*What types of recipes tend to have the most calories?*

This is an important question because food impacts everybody, 
and it is essential to become more aware of what foods to eat while 
maintaining healthy eating standards. Disclaimer: we do not intend our 
results to validate or promote any specific diets. 

In the recipe dataset, there are 83,782 rows where each row represents one recipe. 
Relevant columns include: 
- `name`: the name of the recipe,
- `id`: a unique number representing the recipe, 
- `tags`: a list of tags chosen by the creator,
- `nutrition`: a list of nutritional values in the form—[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)] (PDV—Percent of Daily Value), 
- `n_steps`: the number of steps in the recipe, 
- `n_ingredients`: the number of ingredients used in the recipe, and 
- `description`: an explanation of the recipe provided by some creators. 

In the interactions dataset, there are 731,927 rows where each row represents one review of a recipe.
Relevant columns include:
- `recipe_id`: a unique number representing the recipe,
- `rating`: a rating (1 to 5 stars) for the recipe given by the reviewer.

---
## Cleaning and EDA

#### Data Cleaning Steps:
1. We left merged the recipes and interactions datasets and filled all ratings of 0 with `np.nan`. This is appropriate to do because it is not necessarily the case that the actual review/rating was 0-stars (i.e. the worst rating possible), but the reviewer could be asking a question or state their rating in the review text; additionally, some reviewers stated that the site was not allowing them to click the stars (glitch in the system), and therefore was forced to not provide a rating. 
2. Using the merged dataset, we found the average rating per recipe and added this to the recipes dataset. We then proceed to use the recipes dataset with the average rating column.
3. We added all of the individual nutritional values as their own column using the values from the `nutrition` column. 
4. We identified most of the numerical data type columns from the recipes dataset that were relevant to our analysis. These are: `name`, `id`, `minutes`, `n_steps`, `n_ingredients`, `average_rating`, `calories`, `total_fat_%`, `sugars_%`, `sodium_%`, `protein_%`, `saturated_fat_%`, and `total_carbohydrate_%`. We then proceed to use a dataframe consisting of these columns. 
5. Since the `average_rating` column was a quantitative continuous variable, we decided to bin the values into 0.5-width intervals of ratings and treat it as a categorical variable in the `avg_rating_bins` column.

#### Clean Dataset:

| name                                 |     id |   minutes |   n_steps |   n_ingredients |   average_rating |   calories |   total_fat_% |   sugars_% |   sodium_% |   protein_% |   saturated_fat_% |   total_carbohydrate_% | avg_rating_bins   |
|:-------------------------------------|-------:|----------:|----------:|----------------:|-----------------:|-----------:|--------------:|-----------:|-----------:|------------:|------------------:|-----------------------:|:------------------|
| 1 brownies in the world    best ever | 333281 |        40 |        10 |               9 |                4 |      138.4 |            10 |         50 |          3 |           3 |                19 |                      6 | 4.0 - 4.5         |
| 1 in canada chocolate chip cookies   | 453467 |        45 |        12 |              11 |                5 |      595.1 |            46 |        211 |         22 |          13 |                51 |                     26 | 4.5 - 5.0         |
| 412 broccoli casserole               | 306168 |        40 |         6 |               9 |                5 |      194.8 |            20 |          6 |         32 |          22 |                36 |                      3 | 4.5 - 5.0         |
| millionaire pound cake               | 286009 |       120 |         7 |               7 |                5 |      878.3 |            63 |        326 |         13 |          20 |               123 |                     39 | 4.5 - 5.0         |
| 2000 meatloaf                        | 475785 |        90 |        17 |              13 |                5 |      267   |            30 |         12 |         12 |          29 |                48 |                      2 | 4.5 - 5.0         |

#### Univariate Analysis
<iframe src="assets/calorie_histogram.html" width=800 height=600 frameBorder=0></iframe>
This is a histogram showing the proportion of recipes with a certain range of calories. Each bin is of width 50 calories. The graph is heavily skewed right, due to huge outliers, such as a recipe with ~45,000 calories (wow!). Most of the recipes have 0 to 1,500 calories. 

<iframe src="assets/average_rating_bar.html" width=800 height=600 frameBorder=0></iframe>
This is a bar chart showing the frequency of recipes with an average rating between a certain range. Most recipes are decent; they are on average rated between 4.0 and 5.0 stars.

#### Bivariate Analysis
<iframe src="assets/calorie_rating_box.html" width=800 height=850 frameBorder=0></iframe>
This is a group of box plots showing the distribution of calories for each average rating bin. Observe many outliers especially for higher ratings, but the majority of each bin has a similar distribution.

<iframe src="assets/calories_tag_histogram.html" width=800 height=600 frameBorder=0></iframe>
This is an overlaid histogram showing the distribution of recipes with the `low-calorie` tag compared to recipes without the `low-calorie` tag. As one might expect, the distribution of the `low-calorie` distribution is tighter around its center (i.e. a lower variance) compared to the distribution of recipes without the tag. 

#### Interesting Aggregates

| avg_rating_bins   |1-5 steps|6-10 steps|11-15 steps|16-20 steps|20+ steps|
|:------------------|-------:|-------:|--------:|--------:|-------:|
| 1.0 - 1.5         | 178.2  | 258.1  |  303.35 |  404.3  | 481.5  |
| 1.5 - 2.0         | 119.45 | 305.65 |  338.5  |  559.6  | 411.55 |
| 2.0 - 2.5         | 267.6  | 282.9  |  368    |  410.85 | 407.5  |
| 2.5 - 3.0         | 174.3  | 280.6  |  315.6  |  321.65 | 336.75 |
| 3.0 - 3.5         | 245.5  | 305.2  |  348.6  |  382.3  | 451.7  |
| 3.5 - 4.0         | 248.9  | 306.4  |  332.9  |  364.4  | 459    |
| 4.0 - 4.5         | 235.6  | 299.2  |  359.3  |  388.3  | 440.85 |
| 4.5 - 5.0         | 215.5  | 293.6  |  348.5  |  386.3  | 444.6  |

<iframe src="assets/pivot_bars.html" width=800 height=600 frameBorder=0></iframe>

There is a general trend in an increase of median calories as the number of steps increases. This indicates that recipes with more steps seem to have higher calories according to this dataset. Among each interval of steps, the average rating does not vary much in median of calories, but for each average rating bin (except for 1.5-2.0) there is an increase in median calories with an increase in steps. 

---
## Assessment of Missingness
#### NMAR
We believe that the `average_rating` column is NMAR. The values of `average_rating` may be missing if any rating for a recipe was 0 (which we then replaced with `np.nan`). Thus, it depends on the individual ratings itself.

The `average_rating` column could become MAR if we had included the `rating` column from the interactions dataset. 

#### MAR/MCAR
The column with the most missing values was the `average_rating` column (2,609 missing values!). We determined that it was MAR on `calories` and MCAR on `sodium_%`. 

MAR on `calories`
- $H_0$: The missingness of `average_rating` **does not depend** on `calories`.
- $H_1$: The missingness of `average_rating` **does depend** on `calories`. 
- test statistic: absolute difference in means
- significance level: $\alpha = 0.01$
- conclusion: since the p-value $= 0.003 < \alpha = 0.01$, we reject the $H_0$ and say that **it seems like** the missingness of `average_rating` **depends** on `calories`. 

Given our dataset, the chance of getting the observed statistic or a more extreme absolute difference in means under the null hypothesis is statistically significant. Thus, the number of calories in a recipe affects the missingness of `average_rating`.

MCAR on `sodium_%`
- $H_0$: The missingness of `average_rating` **does not depend** on `sodium_%`.
- $H_1$: The missingness of `average_rating` **does depend** on `sodium_%`. 
- test statistic: absolute difference in means
- significance level: $\alpha = 0.01$
- conclusion: since the p-value $= 0.184 > \alpha = 0.01$, we fail to reject the $H_0$ and say that **it seems like** the missingness of `average_rating` **does not depend** on `sodium_%`. 

Given our dataset, the chance of getting the observed statistic or a more extreme absolute difference in means under the null hypothesis is not statistically significant. This test does not provide any useful information about our question.

---
## Hypothesis Testing
We conducted a permutation test on `n_ingredients` (categorized by "many ingredients") and `calories`. We define "many ingredients" to be `n_ingredients` > 10. 

- $H_0$: The distribution of calories for recipes with 10 or fewer ingredients is **the same** as the distribution of calories for recipes with more than 10 ingredients.
- $H_1$: The distribution of calories for recipes with 10 or fewer ingredients is **different** from the distribution of calories for recipes with more than 10 ingredients.
- test statistic: absolute difference in medians
- significance level: $\alpha = 0.01$
- conclusion: since the p-value $= 0.0 < \alpha = 0.01$, we reject the $H_0$ and say that **it seems like** the distribution of calories for recipes with 10 or fewer ingredients is **different** from the distribution of calories for recipes with more than 10 ingredients. 

Given our dataset, the chance of getting the observed statistic or a more extreme absolute difference in medians under the null hypothesis is statistically significant. Thus, the number of ingredients in a recipe may depend on the number of calories in a recipe. 

Honorable mention:

<div class="table-wrapper" markdown="block">

| name                      |     id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                 | nutrition                                 |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | description                                                                                                                                                                                                                         | ingredients        |   n_ingredients |   average_rating |   calories |   total_fat_% |   sugars_% |   sodium_% |   protein_% |   saturated_fat_% |   total_carbohydrate_% | tags_list                                                                                                                                                                                                              | low-calorie   |
|:--------------------------|-------:|----------:|-----------------:|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------|----------------:|-----------------:|-----------:|--------------:|-----------:|-----------:|------------:|------------------:|-----------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------|
| how to preserve a husband | 447963 |   1051200 |           576273 | 2011-02-01  | ['time-to-make', 'course', 'preparation', 'for-1-or-2', 'low-protein', '5-ingredients-or-less', 'main-dish', '1-day-or-more', 'easy', 'dietary', 'low-sodium', 'low-carb', 'low-in-something', 'number-of-servings'] | [407.4, 57.0, 50.0, 1.0, 7.0, 115.0, 5.0] |         9 | ['be careful in your selection', "don't choose too young", 'when selected , give your entire thoughts to preparation for domestic use', 'some wives insist upon keeping them in a pickle , others are constantly getting them into hot water', 'this may make them sour , hard and sometimes bitter', 'even poor varieties may be made sweet , tender and good by garnishing with patience , well sweetened with love and seasoned with kisses', 'wrap them in a mantle of charity', 'keep warm with a steady fire of domestic devotion and serve with peaches and cream', 'thus prepared , they will keep for years'] | found this in a local wyoming cookbook "a collection of recipes using wine, cordials, and beer" from the broadway liquor store by sara gradin (by the way this particular cookbook was typed up and price on cover was .50 cents!!) | ['cream', 'peach'] |               2 |                5 |      407.4 |            57 |         50 |          1 |           7 |               115 |                      5 | ["'time-to-make", 'course', 'preparation', 'for-1-or-2', 'low-protein', '5-ingredients-or-less', 'main-dish', '1-day-or-more', 'easy', 'dietary', 'low-sodium', 'low-carb', 'low-in-something', "number-of-servings'"] | False         |

</div>