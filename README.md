# Calorie Hunters :D
This is a project for the Winter 2023 offering of DSC 80 at UC San Diego, conducting EDA on recipe and review data from food.com. 

Members: Kurumi Kaneko, Candus Shi

## Introduction
For this project we were provided two datasets from food.com. 
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

## Cleaning and EDA

## Assessment of Missingness

## Hypothesis Testing
