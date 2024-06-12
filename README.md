# Recipes and Ratings Project
## Overview
This is our final project for DSC 80, focussing on the relationship between the ratings and the overall difficulty of making that meal. 
## Introduction
As college students, we barely have time during the day to cook fresh meals that we have grown accustommed to throughout our lives, heck we barely know how to cook! Plus the rising costs of doordashing, uber eating or eating outside will definitely drain our bank accounts. With the average cost of fastfood rising by around 7% in April according to data from Kalinowski Equity Research, we thought we could investigate the relationship between the difficulty in making a meal and its overall rating. Our aim is to identify meals which are relatively simple to make and are tasty. To do so, we have found a dataset containing recipes orginally scraped from food.com and another dataset which contains the ratings and reviews given by users, also from food.com. <br>
<br>
The recipes dataset has 83782 rows with the following 12 columns:<br>
<iframe
  src="assets/recipes-table.html"
  width="700"
  height="450"
  frameborder="0"
></iframe> 

The second dataset, interactions, has 731927 rows with the following 5 columns:<br>
<iframe
  src="assets/interactions-table.html"
  width="700"
  height="300"
  frameborder="0"
></iframe> 

Based on these two datasets we will investigate the relationship between the difficulty in making a recipe and its rating, to identify easy to make recipes for college students which also taste good. The columns relevant to finding out the difficulty of a recipe are the number of steps, and the time taken to make the recipe. 

## Data Cleaning and Exploratory Data Analysis
We took the following steps to clean and merge the recipes and interactions datasets:- <br>
1. We removed any duplicated to avoid any biases or any errors in future hypothesis and permutation testing and predictive models
2. Next, to not skew statistics such as the average time taken for a dish or the average number of steps we have removed recipes with more than 100 steps and recipes that takes more than a 1000 intues to pepare
3. Then we renamed the 'id' column in recipes to to 'recipes_id' and did a left merge on recipes, and interactions, giving us a dataframe with 234429 rows
4. On this merged dataframe, we substituted the 0s in the rating column with np.nan as the rating is from 1 to 5, indicating that 0 stands for missing values and we do not want these values to skew the average rating of a recipe
5. Then we computed the average rating per recipe and added it as a column in our merged dataframe. We kept both rating and average rating as there are 15036 rows with np.nan in the rating column, which is roughly 6.4% of our dataframe.Keeping both columns allows us to compare future statistical analysis with and without mean imputation. 
6. Then we parsed the nutrition column into individual values, added each value into our dataframe. These values were originally stored as a string which was a list, so had to call the str object, strip the parentheses, split the string, add them to our dataframe and then convert them to float.
7.  We also categorized each recipe into 'Easy', 'Medium' or 'Hard' based on the number of steps required and the overall time taken to make that recipe. 
8.  We also added another column 'avg_time_per_step' which divides the total number of minutes to the number of steps as there might be some recipes that have few steps, making it seem like they are easy to make but may take a lot of time. <br>

After all of these steps, we had a dataframe with 234429 rows and 26 columns:<br>
"|    | name                                 |   recipe_id |   minutes | tags                                                                                                                                                                                                                        |   n_steps |   n_ingredients |   rating |   rating_average |   calories |   total_fat_PDV |   sugar_PDV |   sodium_PDV |   protein_PDV |   saturated_fat_PDV |   carbohydrates_PDV | difficulty   |   avg_time_per_step |\n|---:|:-------------------------------------|------------:|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|----------------:|---------:|-----------------:|-----------:|----------------:|------------:|-------------:|--------------:|--------------------:|--------------------:|:-------------|--------------------:|\n|  0 | 1 brownies in the world    best ever |      333281 |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] |        10 |               9 |        4 |                4 |      138.4 |              10 |          50 |            3 |             3 |                  19 |                   6 | Medium       |             4       |\n|  1 | 1 in canada chocolate chip cookies   |      453467 |        45 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               |        12 |              11 |        5 |                5 |      595.1 |              46 |         211 |           22 |            13 |                  51 |                  26 | Hard         |             3.75    |\n|  2 | 412 broccoli casserole               |      306168 |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 |               9 |        5 |                5 |      194.8 |              20 |           6 |           32 |            22 |                  36 |                   3 | Medium       |             6.66667 |\n|  3 | 412 broccoli casserole               |      306168 |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 |               9 |        5 |                5 |      194.8 |              20 |           6 |           32 |            22 |                  36 |                   3 | Medium       |             6.66667 |\n|  4 | 412 broccoli casserole               |      306168 |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 |               9 |        5 |                5 |      194.8 |              20 |           6 |           32 |            22 |                  36 |                   3 | Medium       |             6.66667 |"



