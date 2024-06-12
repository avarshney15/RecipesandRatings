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
<table>
  <thead>
    <tr>
      <th></th>
      <th>name</th>
      <th>recipe_id</th>
      <th>minutes</th>
      <th>tags</th>
      <th>n_steps</th>
      <th>n_ingredients</th>
      <th>rating</th>
      <th>rating_average</th>
      <th>calories</th>
      <th>total_fat_PDV</th>
      <th>sugar_PDV</th>
      <th>sodium_PDV</th>
      <th>protein_PDV</th>
      <th>saturated_fat_PDV</th>
      <th>carbohydrates_PDV</th>
      <th>difficulty</th>
      <th>avg_time_per_step</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1 brownies in the world best ever</td>
      <td>333281</td>
      <td>40</td>
      <td>['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings']</td>
      <td>10</td>
      <td>9</td>
      <td>4</td>
      <td>4</td>
      <td>138.4</td>
      <td>10</td>
      <td>50</td>
      <td>3</td>
      <td>3</td>
      <td>19</td>
      <td>6</td>
      <td>Medium</td>
      <td>4</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1 in canada chocolate chip cookies</td>
      <td>453467</td>
      <td>45</td>
      <td>['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']</td>
      <td>12</td>
      <td>11</td>
      <td>5</td>
      <td>5</td>
      <td>595.1</td>
      <td>46</td>
      <td>211</td>
      <td>22</td>
      <td>13</td>
      <td>51</td>
      <td>26</td>
      <td>Hard</td>
      <td>3.75</td>
    </tr>
    <tr>
      <td>2</td>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']</td>
      <td>6</td>
      <td>9</td>
      <td>5</td>
      <td>5</td>
      <td>194.8</td>
      <td>20</td>
      <td>6</td>
      <td>32</td>
      <td>22</td>
      <td>36</td>
      <td>3</td>
      <td>Medium</td>
      <td>6.66667</td>
    </tr>
    <tr>
      <td>3</td>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']</td>
      <td>6</td>
      <td>9</td>
      <td>5</td>
      <td>5</td>
      <td>194.8</td>
      <td>20</td>
      <td>6</td>
      <td>32</td>
      <td>22</td>
      <td>36</td>
      <td>3</td>
      <td>Medium</td>
      <td>6.66667</td>
    </tr>
    <tr>
      <td>4</td>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']</td>
      <td>6</td>
      <td>9</td>
      <td>5</td>
      <td>5</td>
      <td>194.8</td>
      <td>20</td>
      <td>6</td>
      <td>32</td>
      <td>22</td>
      <td>36</td>
      <td>3</td>
      <td>Medium</td>
      <td>6.66667</td>
    </tr>
  </tbody>
</table>




