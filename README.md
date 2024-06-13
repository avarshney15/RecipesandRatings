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
#### Impact of n_steps on Missingness of Rating
We will now run a permutation test to analyze whether the missingness of the 'rating' column depends on 'n_steps' which is the total number of ingredients required to make the recipe. 

**Null Hypothesis**: The missingness of ratings does not depend on the number of steps required to make the recipe.

**Alternate Hypothesis**: The missingness of ratings does depend on the number of steps required to make the recipe.

**Test Statistic**: The difference in mean in the number of steps of the distribution of the group with missing ratings and the distribution of the group without missing ratings.
<br>
We will run a permutation test over a 1000 iterations where we will shuffle the 'rating_missing' column we have temporarily added and compare our test statistic with our observed value of 1.3386412335909217. 
<br>

**Significance Level**: 0.05
<iframe
  src="assets/missingness1.html"
  width="700" 
  height="450"  
  frameborder="0"
  style="width: 100%; height: 100%; min-width: 700px; min-height: 450px;"  
></iframe>
Based on the p-value calculated 0.0, we have to reject the null hypothesis proving that the values of missingness of ratings does depend on the number of steps.

#### Impact of Minutes on Missingness of Rating
We will now run a permutation test to analyze whether the missingness of the 'rating' column depends on 'minutes' which is the total number of minutes taken to make the recipe. 

**Null Hypothesis**: The missingness of ratings does not depend on the number of minutes required to make the recipe.

**Alternate Hypothesis**: The missingness of ratings does depend on the number of minutes required to make the recipe.

**Test Statistic**: The difference in mean in the number of minutes of the distribution of the group with missing ratings and the distribution of the group without missing ratings.
<br>
We will run a permutation test over a 1000 iterations where we will shuffle the 'rating_missing' column we have temporarily added and compare our test statistic with our observed value of 51.45237039852127. 
<br>

**Significance Level**: 0.05
<iframe
  src="assets/missingness2.html"
  width="700" 
  height="450"  
  frameborder="0"
  style="width: 100%; height: 100%; min-width: 700px; min-height: 450px;"  
></iframe>
Based on the p-value calculated 0.107, we fail to reject the null hypothesis and connot prove that the values of missingness of ratings does depend on the minutes taken to make the recipe. 

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
  width="700" 
  height="450"  
  frameborder="0"
  style="width: 100%; height: 100%; min-width: 700; min-height: 450px;"  
></iframe>

**Results**:
Observed Difference in Mean Ratings: 0.03417508887158416
<br>

**P-value**: 0.0
<br>

**Conclusion**:Based on the p-value of 0.0, we reject the null hypothesis. This indicates that there is a significant difference in ratings between recipes that take less time and those that take more time. Specifically, the results suggest that people tend to rate recipes that take less time higher than those that take more time.
The extremely low p-value (0.0) strongly indicates that the observed difference in mean ratings is not due to random chance. This provides robust evidence that preparation time significantly influences recipe ratings, validating our alternative hypothesis.

## Framing a Prediction Problem
For our prediction problem, we want to predict the time taken to make a recipe. This prediction problem is a regression task, where the goal is to predict a continuous numerical value representing the time (in minutes) required to prepare a recipe.
<br>

**Prediction Problem**: Predict the time taken to make a recipe.

**Type of Problem**: Regression

**Response Variable**: The response variable is minutes, which represents the time taken to make a recipe.

**Justification for Choosing the Response Variable**:The time required to prepare a recipe is a critical piece of information for college students especially	during final week. Knowing how long a recipe will take can help users plan their meals more effectively and choose recipes that fit their available time. Additionally, recipe creators and websites can use this information to categorize and recommend recipes based on the user's time constraints.
<br>


**Test Statistic**: For evaluating our regression model, we will use the Mean Squared Error (MSE) and the R-squared (R²) score.
<br>

**Mean Squared Error (MSE)**: MSE measures the average squared difference between the actual and predicted values. It is a commonly used metric for regression problems as it gives a sense of how far the predicted values are from the actual values. Lower MSE values indicate better model performance.
<br>

**R-squared (R²) Score**: The R² score, also known as the coefficient of determination, indicates the is the proportion of variance in  
y that the linear model explains. An R² score closer to 1 implies that the model explains a large portion of the variance in the response variable. Having a high R² score would mean that our features do a good job of predicting the minutes. 
<br>
We chose MSE because it provides a clear measure of the average error in the predictions, penalizing larger errors more significantly, forcing our model to be really accurate as our dataframe has over 230,000 rows and the prediction must be close for all rows to reduce the MSE. The R² score is chosen to provide an understanding of how well the model explains the variability in the response variable.
<br>
We will use the features and dataframe described in the Data Cleaning section and will divide the dataset into a train test split to ensure fair evaluation of our Baseline and Final model. 

## Baseline Model
For our baseline model, we trained a linear regression model to predict the time required to prepare a recipe (minutes) using the number of steps and rating as features. The steps involved in creating this model are as follows:
<br>

**Features**:
We used two features:
1. n_steps: The number of steps in the recipe (quantitative).
2. rating: The user rating for the recipe (quantitative).

Target:
The target variable we are predicting is minutes (quantitative).
<br>

**Data Preparation**:
We handled missing values by dropping rows with missing values in the selected columns.
We scaled the numerical features using standard scaling.
<br>

**Model**:
We used a linear regression model.
Pipeline Implementation:
We implemented all the data preprocessing steps and model training within a single sklearn pipeline to ensure a streamlined and reproducible workflow. This pipeline included scaling the features and training the linear regression model.
<br>

**Model Performance**:
We evaluated the model's performance on unseen data using a train-test split. Here are the results:
- Mean Squared Error (MSE): 656690.5075580779
- R^2 Score: 0.002430478288256044
<br>

**Conclusion**:
The performance metrics indicate that the baseline model is not performing well. The high MSE suggests that the predictions are significantly off from the actual values, and the very low R^2 score indicates that the model is not effectively explaining the variance in the target variable (minutes).
This result suggests that the number of steps and the rating alone are insufficient to predict the time required to prepare a recipe accurately. It is likely that other factors, such as the complexity of the steps, the type of cuisine, or specific ingredients, play a more significant role in determining the preparation time.
