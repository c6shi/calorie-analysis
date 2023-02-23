<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
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
***What types of recipes tend to have the most calories?***

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
5. Since the `average_rating` column was a quantitative continuous variable, we decided to bin the values into 0.5-width intervals of ratings and treat it as a categorical variable in the `avg_rating_bins` column. One limitation of binning average rating is that we lose data granularity so the following analysis may not produce the most accurate results. 

#### Clean Dataset:

<div class="table-wrapper" markdown="block">

| name                                 |     id |   minutes |   n_steps |   n_ingredients |   average_rating |   calories |   total_fat_% |   sugars_% |   sodium_% |   protein_% |   saturated_fat_% |   total_carbohydrate_% | avg_rating_bins   |
|:-------------------------------------|-------:|----------:|----------:|----------------:|-----------------:|-----------:|--------------:|-----------:|-----------:|------------:|------------------:|-----------------------:|:------------------|
| 1 brownies in the world    best ever | 333281 |        40 |        10 |               9 |                4 |      138.4 |            10 |         50 |          3 |           3 |                19 |                      6 | 4.0 - 4.5         |
| 1 in canada chocolate chip cookies   | 453467 |        45 |        12 |              11 |                5 |      595.1 |            46 |        211 |         22 |          13 |                51 |                     26 | 4.5 - 5.0         |
| 412 broccoli casserole               | 306168 |        40 |         6 |               9 |                5 |      194.8 |            20 |          6 |         32 |          22 |                36 |                      3 | 4.5 - 5.0         |
| millionaire pound cake               | 286009 |       120 |         7 |               7 |                5 |      878.3 |            63 |        326 |         13 |          20 |               123 |                     39 | 4.5 - 5.0         |
| 2000 meatloaf                        | 475785 |        90 |        17 |              13 |                5 |      267   |            30 |         12 |         12 |          29 |                48 |                      2 | 4.5 - 5.0         |

</div>

#### Univariate Analysis
<iframe src="assets/calorie_histogram.html" width=1000 height=600 frameBorder=0></iframe>
This is a histogram showing the proportion of recipes with a certain range of calories. Each bin is of width 50 calories. The graph is heavily skewed right, due to huge outliers, such as a recipe with ~45,000 calories (wow!). Most of the recipes have 0 to 2,500 calories.
<iframe src="assets/calorie_histogram_zoom.html" width=1000 height=600 frameBorder=0></iframe>
The histogram above shows the bulk of the data (0 to 2,500 calories) in a more visually appealing manner.

<iframe src="assets/average_rating_bar.html" width=1000 height=600 frameBorder=0></iframe>
This is a bar chart showing the frequency of recipes with an average rating between a certain range. Most recipes are decent; they are on average rated between 4.0 and 5.0 stars.

#### Bivariate Analysis
<iframe src="assets/calorie_rating_box.html" width=1000 height=850 frameBorder=0></iframe>
This is a group of box plots showing the distribution of calories for each average rating bin. Observe that there are many outliers especially for higher ratings, but most of the bins have a similar distribution.
<iframe src="assets/calorie_rating_box_zoom.html" width=1000 height=850 frameBorder=0></iframe>
The box plot above shows the bulk of the data (0 to 1,500 calories) in a more visually appealing manner. The interquartile range of all bins is around 150 calories to 500 calories. Note that there is a lot more data for 4.0-4.5 and 4.5-5.0 bins as shown in the bar chart in the above section. This means that there is an uneven distribution of recipes among the average rating bins, so it may not be correct to compare the box plots equally on their own.

<iframe src="assets/calories_tag_histogram.html" width=1000 height=600 frameBorder=0></iframe>
This is an overlaid histogram showing the distribution of recipes with the `low-calorie` tag compared to recipes without the `low-calorie` tag. As one might expect, the distribution of the `low-calorie` distribution is tighter around its center (i.e. has a lower variance) compared to the distribution of recipes without the tag.
<iframe src="assets/calories_tag_histogram_zoom.html" width=1000 height=600 frameBorder=0></iframe>
The histogram above shows the bulk of the data (0 to 1,500 calories) in a more visually appealing manner.

#### Interesting Aggregates

Below is a pivot table of median number of calories per number of steps and average rating bins. 

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

There is a general trend in an increase of median calories as the number of steps increases. This indicates that recipes with more steps seem to have higher calories according to this dataset. Among each interval of steps, the average rating does not vary much in median of calories, but for each average rating bin (except for 1.5-2.0) there is an increase in median calories with an increase in steps. This is more easily visualized in the bar chart below.

<iframe src="assets/pivot_bars.html" width=1000 height=600 frameBorder=0></iframe>


---
## Assessment of Missingness
#### NMAR
We believe that the `average_rating` column is NMAR. The values of `average_rating` may be missing if any rating for a recipe was 0 (which we then replaced with `np.nan` in the data cleaning steps) as this would affect the computation of the average because taking the average of values where at least one value is nan results in an average that is also nan. Thus, it depends on the individual ratings itself which in turn makes the missingness of `average_rating` dependent on its values. Hence, it is NMAR. 

The `average_rating` column could become MAR if we had included the `rating` column from the interactions dataset. 

#### MAR/MCAR
The column with the most missing values was the `average_rating` column (2,609 missing values!). We determined that it was MAR on `calories` and MCAR on `sodium_%`. 

**MAR on `calories`**
- $H_0$: The missingness of `average_rating` **does not depend** on `calories`.
- $H_1$: The missingness of `average_rating` **does depend** on `calories`. 
- test statistic: absolute difference in means
- significance level: $\alpha = 0.01$
- conclusion: since the p-value $= 0.0 < \alpha = 0.01$, we reject the $H_0$ and say that **it seems like** the missingness of `average_rating` **depends** on `calories`. 

<iframe src="assets/mar_histogram.html" width=1000 height=600 frameBorder=0></iframe>

Given our dataset, the chance of getting the observed statistic or a more extreme absolute difference in means under the null hypothesis is statistically significant. Thus, the number of calories in a recipe affects the missingness of `average_rating`.


**MCAR on `sodium_%`**
- $H_0$: The missingness of `average_rating` **does not depend** on `sodium_%`.
- $H_1$: The missingness of `average_rating` **does depend** on `sodium_%`. 
- test statistic: absolute difference in means
- significance level: $\alpha = 0.01$
- conclusion: since the p-value $= 0.894 > \alpha = 0.01$, we fail to reject the $H_0$ and say that **it seems like** the missingness of `average_rating` **does not depend** on `sodium_%`. 

<iframe src="assets/mcar_histogram.html" width=1000 height=600 frameBorder=0></iframe>

Given our dataset, the chance of getting the observed statistic or a more extreme absolute difference in means under the null hypothesis is not statistically significant. This test does not provide any useful information about our question.

---
## Hypothesis Testing
We conducted a permutation test on `n_ingredients` (categorized by "many ingredients") and `calories`. We define "many ingredients" to be `n_ingredients` > 10. We are doing this test to see whether the number of ingredients in a recipe can tell us if such recipes tend to have higher calories. We chose 10 ingredients as our threshold by looking at the distribution of number of ingredients (in the histogram below) and eye-balling an approximate balancing point. 

<iframe src="assets/many_ingredients_histogram.html" width=1000 height=600 frameBorder=0></iframe>

- $$H_0$$: The distribution of calories for recipes with 10 or fewer ingredients is **the same** as the distribution of calories for recipes with more than 10 ingredients.
- $H_1$: The distribution of calories for recipes with 10 or fewer ingredients is **different** from the distribution of calories for recipes with more than 10 ingredients. Specifically, recipes with more than 10 ingredients have more calories than recipes with 10 or fewer ingredients. 
- test statistic: difference in group medians
- significance level: $\alpha = 0.01$
- conclusion: since the p-value $= 0.0 < \alpha = 0.01$, we reject the $H_0$ and say that **it seems like** the distribution of calories for recipes with 10 or fewer ingredients is **different** from the distribution of calories for recipes with more than 10 ingredients. 

<iframe src="assets/permutation_histogram.html" width=1000 height=600 frameBorder=0></iframe>

Given our dataset, the chance of getting the observed statistic or a more extreme difference in group medians under the null hypothesis is statistically significant. We can also say that recipes with more than 10 ingredients have more calories than recipes with 10 or fewer ingredients. Thus, to answer our main question, recipes with higher calories may have a higher number of ingredients. 

---
#### Honorable mention:

<div class="table-wrapper" markdown="block">

| name                      |     id |   minutes |   contributor_id | submitted   | tags                                                              | nutrition                                 |   n_steps | steps                                                                                                                           | ingredients        |   n_ingredients |   average_rating |
|:--------------------------|-------:|----------:|-----------------:|:------------|:------------------------------------------------------------------|:------------------------------------------|----------:|:--------------------------------------------------------------------------------------------------------------------------------|:-------------------|----------------:|-----------------:|
| how to preserve a husband | 447963 |   1051200 |           576273 | 2011-02-01  | ['time-to-make', ..., 'for-1-or-2', ..., 'low-in-something', ...] | [407.4, 57.0, 50.0, 1.0, 7.0, 115.0, 5.0] |         9 | [..., "don't choose too young", ..., 'keep warm with a steady fire of domestic devotion and serve with peaches and cream', ...] | ['cream', 'peach'] |               2 |                5 |

</div>