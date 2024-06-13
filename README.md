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
### Data Cleaning
We took the following steps to clean and merge the recipes and interactions datasets:- <br>
1. We removed any duplicated to avoid any biases or any errors in future hypothesis and permutation testing and predictive models
2. Next, to not skew statistics such as the average time taken for a dish or the average number of steps we have removed recipes with more than 100 steps and recipes that takes more than a 1000 intues to pepare
3. Then we renamed the 'id' column in recipes to to 'recipes_id' and did a left merge on recipes, and interactions, giving us a dataframe with 234429 rows
4. On this merged dataframe, we substituted the 0s in the rating column with np.nan as the rating is from 1 to 5, indicating that 0 stands for missing values and we do not want these values to skew the average rating of a recipe
5. Then we computed the average rating per recipe and added it as a column in our merged dataframe. We kept both rating and average rating as there are 15036 rows with np.nan in the rating column, which is roughly 6.4% of our dataframe.Keeping both columns allows us to compare future statistical analysis with and without mean imputation. 
6. Then we parsed the nutrition column into individual values, added each value into our dataframe. These values were originally stored as a string which was a list, so had to call the str object, strip the parentheses, split the string, add them to our dataframe and then convert them to float.
7.  We also categorized each recipe into 'Easy', 'Medium' or 'Hard' based on the number of steps required and the overall time taken to make that recipe. 
8.  We also added another column 'avg_time_per_step' which divides the total number of minutes to the number of steps as there might be some recipes that have few steps, making it seem like they are easy to make but may take a lot of time. <br>

After all of these steps, we had a dataframe with 234429 rows and 26 columns. To present the relevant data left, some columns have been removed to ensure neatness and maintain readability. Here are the first five rows of our merged dataframe:<br>
Please scroll down and to the right to see all of the columns and rows<br>
<iframe
  src="assets/new-table-head.html"
  width="1200" 
  height="500"  
  frameborder="0"
  style="width: 100%; height: 100%; min-width: 1200px; min-height: 600px;"  
></iframe>

### Univariate Analysis
For our univariate analysis, we examined the overall distribution of the ratings of recipes in our dataframe and the distribution for the number of steps of all recipes. This would show us the overall trend in the satisfaction of the users and the number of steps taken to make the recipes, helping us further analyze the data in our bivariate analysis and hypothesis testing. 
<iframe
  src="assets/univariate-ratings.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  
This histograms presents the distribution of rating of all recipes from 1 to 5, with the data largely skewing towards the right indicating that a large proportion of the recipes in the dataframe are given 4 and 5 stars, showing a high level of user statisfaction. 
<iframe
  src="assets/univariate-nsteps.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  
This histograms presents the distribution of the number of steps of all recipes from 1 to 100, with the data largely skewing towards the left indicating that a large proportion of the recipes in the dataframe take less than 40 steps to make and only a handful of recipes take more than 60 steps, giving us further insight on thier difficult levels. 

### Bivariate Analysis
For our bivariate analysis, we examined the relationship between the number of steps and the rating to identify any trends between the two variables using a scatterplot. We also plotted a bar chart between the ratings and the average time taken to prepare recipes with that rating. 
<iframe
  src="assets/bivariate-nstepsandrating.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  
The scatterplot above, shows the distribution of the number of steps taken, broken down by the rating recieved form 1 to 5. From our univariate analysis we found out that a large proportion of the recipes take less than 40 steps to make, however, we can now see that recipes even with a low rating take less than 40 steps. Infact, as the rating increases, the number of steps also increases as there are a large number of recipes with a rating of 5 which take more than 40 steps while there are almost no recipes with a rating of 1 which taken more than 40 steps. 

<iframe
  src="assets/bivariate-boxgraph.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  
The bar graph above, shows the average time taken for recipes to be made versus the rating recieved. We can see that the average time taken to produce a recipe with rating 1 alomost takes the same amount of time as producing a recipe with rating 5, with the difference between the two being only 6.9 minutes. What's interesting is that 87.4 minutes to create a recipe with rating 3 and 91.5  to create a recipe with rating 4. 

### Interesting Aggregates
For this section, we grouped our data to find the relationship between the number of steps and the average rating by calculating the mean, median, standard deviation, minimum value and maximum value. We have the head and the tail for our resutling  dataframe below:
<iframe
  src="assets/interesting_agg_tail.html"
  width="1200" 
  height="200"  
  frameborder="0"
  style="width: 100%; height: 100%; min-width: 1000px; min-height: 350px;"  
></iframe>
In the head, we can see that the minimum rating is 1 while the maximum and the median is 5, suggesting that even when the number of steps are generally less, there are mostly recipes with 5 star ratings. The standard deviation is also between 0.6 and 0.7 which is large as this suggests 12% to 14% variation in ratings. 
<iframe
  src="assets/interesting_agg_tail.html"
  width="1200" 
  height="200"  
  frameborder="0"
  style="width: 100%; height: 100%; min-width: 1000px; min-height: 350px;"  
></iframe>
In the tail, we can see that the minimum rating is 3 only when n_steps is 88, otherwise the minimum and maximum is 5. This suggests that when the number of steps are generally more, there are mostly recipes with 5 star ratings as well. The standard deviation is also 0 for 4 of the 5 rows showing that there are only 5 star recipes when the number of steps increase.

## Assessment of Missingness
In our merged dataframe, there were missing values in 'rating', 'date' and 'review'. Of these 3 columns, 'rating' is the most relevant for our project, hence we will analyze the missingness of rating. 
### NMAR Analysis
We believe that the missingness 'review' is NMAR as it takes a lot of time and effort to write a review as opposed to just leaving a rating. Users would be more likely to leave a review if they really liked or really hated a particular recipe. 

### Missingness Dependancy
We will now run a permutation test to analyze whether the missingness of the 'rating' column depends on 'n_ingredients' which is theo total number of ingredients required to make the recipe. 


We moved on to examine the missingness of 'rating' in the merged DataFrame by testing the dependency of its missingness. We are investigating whether the missiness in the 'rating' column depends on the column 'prop_sugar', which is the proportion of sugar out of the total calories, or the column 'n_steps', which is the number of steps of the recipe.

Null Hypothesis: The missingness of ratings does not depend on the number of steps required to make the recipe.

Alternate Hypothesis: The missingness of ratings does depend on the number of ingredients required to make the recipe.

Test Statistic: The difference of mean in the number of steps of the distribution of the group with missing ratings and the distribution of the group without missing ratings.

Significance Level: 0.05
<iframe
  src="assets/missingness1.html"
  width="700" 
  height="400"  
  frameborder="0"
  style="width: 100%; height: 100%; min-width: 700px; min-height: 40px;"  
></iframe>



## Hypothesis Testing
### Rating vs Minutes
In this step, we conducted a hypothesis test to determine whether people rate recipes that take less time higher than recipes that take more time. We used the difference in mean ratings between the two groups (recipes taking less time and recipes taking more time) as our test statistic.

**Null Hypothesis**: People rate all recipes on the same scale. Any observed difference in ratings between recipes that take less time and those that take more time is due to random chance.
<br>

**Alternative Hypothesis**: People rate recipes that take less time higher than recipes that take more time.
<br>

**Justification for Choice of Hypotheses**:
These hypotheses are directly aligned with our research question, which seeks to understand if the time taken to prepare a recipe influences its rating. By testing whether there is a significant difference in ratings based on preparation time, we can draw conclusions about user preferences related to the convenience and effort required for different recipes. Our observed statistic is 0.03417508887158416 and we think that pople might rate less time taking recipes worse as the effort made to cook is lower consequently making the user satisfaction lower. 

**Test Statistic**: We used the difference in mean ratings as our test statistic to measure the observed difference between recipes taking less time and recipes taking more time.

**Justification for Choice of Test Statistic**:
The mean rating is a straightforward and interpretable metric that directly reflects user satisfaction and perception of the recipes. Using the difference in mean ratings allows us to quantify the impact of preparation time on user ratings, providing clear and actionable insights.
<iframe
  src="assets/step4.html"
  width="1200" 
  height="200"  
  frameborder="0"
  style="width: 100%; height: 100%; min-width: 1000px; min-height: 350px;"  
></iframe>

**Results**:
Observed Difference in Mean Ratings: 0.03417508887158416
<br>

**P-value**: 0.0
<br>

**Conclusion**:Based on the p-value of 0.0, we reject the null hypothesis. This indicates that there is a significant difference in ratings between recipes that take less time and those that take more time. Specifically, the results suggest that people tend to rate recipes that take less time higher than those that take more time.
The extremely low p-value (0.0) strongly indicates that the observed difference in mean ratings is not due to random chance. This provides robust evidence that preparation time significantly influences recipe ratings, validating our alternative hypothesis.
