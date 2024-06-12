# [GitHub Repository Here](https://github.com/jiy016/Recipe-Rating-Analysis-Prediction/tree/main)

# Overview
This project is part of DSC80 course at UCSD, which is investigating the influence of different factors on the average rating of recipes.

Authors: Lora Bednarek & Jiaxin Yang

### Introduction

We love food and cooking. However, as college students, we are both very busy and are tired of making a recipe we found online and not enjoying the meal. With this in mind, we want to know if there are certain types of recipes and qualities of reviews that could lead us in the direction of picking a good recipe. We used two datasets obtained from [food.com](https://www.food.com) to explore this.

We downloaded the csv files from the [link](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view) provided on the course website. 

Our dataset includes information about recipes and their corresponding ratings, split into two main csv files: one containing recipe information (*RAW_recipes.csv*), and another file containing user interactions like ratings and reviews (*RAW_interactions.csv*).

- *RAW_recipes.csv* contains recipes.
- *RAW_interactions.csv* contains reviews and ratings submitted for the recipes in *RAW_recipes.csv*.

For **RAW_recipes** dataframe, it has 83782 rows × 12 columns.
Each row has the following columns:

**Column**|**Description**
---|---
*'name'*|Recipe name
*'id'*|Recipe ID
*'minutes'*|Minutes to prepare recipe
*'contributor_id'*|User ID who submitted this recipe
*'submitted'*|Date recipe was submitted
*'tags'*|Food.com tags for recipe
*'nutrition'*|Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”
*'n_steps'*|Number of steps in recipe
*'steps'*|Text for recipe steps, in order
*'description'*|User-provided description


For **RAW_interactions** dataframe, it has 731927 rows × 5 columns. This is having much more rows because there could be multiple ratings and reviews for a single recipe.
Each row has the following columns:

| **Column**|**Description** |
| --- | --- |
| *'user_id'* | User ID |
| *'recipe_id'* | Recipe ID |
| *'date'* | Date of interaction |
| *'rating'* | Rating given |
| *'review'* | Review text |


Given these datasets, our primary question is: **Are there certain factors that play into ratings of a recipe? If so, can we predict the rating of a recipe given other factors?**


### Data Cleaning and Exploratory Data Analysis

First, we decide to merge those two dataframes based on *recipe_id* and get an combined dataframe. This is what it looks like:


|    | name                              |     id |   minutes |   contributor_id | submitted   | tags                              | nutrition                                    |   n_steps | steps                             | description                       | ingredients                       |   n_ingredients |          user_id |   recipe_id | date       |   rating | review                            |
|---:|:----------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------|:---------------------------------------------|----------:|:----------------------------------|:----------------------------------|:----------------------------------|----------------:|-----------------:|------------:|:-----------|---------:|:----------------------------------|
|  0 | 1 brownies in the world best... | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-t... | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | ['heat the oven to 350f and ar... | these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 | 386585           |      333281 | 2008-11-19 |        4 | These were pretty good, but to... |
|  1 | 1 in canada chocolate chip coo... | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-t... | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | ['pre-heat oven the 350 degree... | this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 | 424680           |      453467 | 2012-01-26 |        5 | Originally I was gonna cut the... |
|  2 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |  29782           |      306168 | 2008-12-31 |        5 | This was one of the best brocc... |
|  3 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |      1.19628e+06 |      306168 | 2009-04-13 |        5 | I made this for my son's first... |
|  4 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 | 768828           |      306168 | 2013-08-02 |        5 | Loved this.  Be sure to comple... |

- We replaced all "0" ratings with np.NaN to not skew the average ratings to lower. 
- We added two columns, one contains the number of reviews for each recipe (*num_reviews*), and the other one contains the every review for each recipe (*reviews*). 
- We split the *nutrition* column into 7 columns, each containing the amount of corresponding nutrition, then dropped the original *nutrition* column.
- We calculated the *average rating* for each recipe.
- We added the *season* column by the *sumbitted* time, indicating the season the recipe was posted. 
  - Spring: 3-5. Summer: 6-8. Fall: 7-9. Winter: 12-2 (month).

After adding the following columns:

| **Column**|**Description** |
| --- | --- |
| *'num_reviews'* | The number of reviews of this recipe |
| *'reviews'* | All of the written reviews of this recipe |
| *'average_ratings'* | Average rating of this recipe |
| *'calories '* | The amount of calories |
| *'total fat'* | The amount of total fat |
| *'sugar '* | The amount of sugar |
| *'sodium  '* | The amount of sodium |
| *'protein  '* | The amount of protein |
| *'saturated fat '* | The amount of saturated fat |
| *'carbohydrates  '* | The amount of carbohydrates |
| *'season  '* | The season of the submittion time |


This is the dataframe after the data cleaning. It contains a row for each recipe:

|    | name                                 |     id |   minutes |   contributor_id | submitted   | tags                              |   n_steps | steps                             | description                       | ingredients                       |   n_ingredients |   average_ratings |   num_reviews | reviews                           |   calories |   total fat |   sugar |   sodium |   protein |   saturated fat |   carbohydrates | season   |
|---:|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------|----------:|:----------------------------------|:----------------------------------|:----------------------------------|----------------:|------------------:|--------------:|:----------------------------------|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|:---------|
|  0 | 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-t... |        10 | ['heat the oven to 350f and ar... | these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 |                 4 |             1 | These were pretty good, but to... |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 | fall     |
|  1 | 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-t... |        12 | ['pre-heat oven the 350 degree... | this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 |                 5 |             1 | Originally I was gonna cut the... |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 | spring   |
|  2 | 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |                 5 |             4 | This was one of the best brocc... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | spring   |
|  3 | millionaire pound cake               | 286009 |       120 |           461724 | 2008-02-12  | ['time-to-make', 'course', 'cu... |         7 | ['freheat the oven to 300 degr... | why a millionaire pound cake? ... | ['butter', 'sugar', 'eggs', 'a... |               7 |                 5 |             1 | don't let the calories and fat... |      878.3 |          63 |     326 |       13 |        20 |             123 |              39 | winter   |
|  4 | 2000 meatloaf                        | 475785 |        90 |          2202916 | 2012-03-06  | ['time-to-make', 'course', 'ma... |        17 | ['pan fry bacon , and set asid... | ready, set, cook! special edit... | ['meatloaf mixture', 'unsmoked... |              13 |                 5 |             2 | Delicious!!!!! -- the goat che... |      267   |          30 |      12 |       12 |        29 |              48 |               2 | spring   |

83782 rows × 22 columns

This **recipes** dataframe is easier to process. We will use this dataframe to do the further works.

#### Univariate Analysis

We first explored the distribution of ratings.
<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Generally, the recipes are getting pretty good ratings. But, there is a lot of variability.

#### Bivariate Analysis

We wanted to examine the relationship between number of reviews and the average rating of a recipe. It appears that as ratings goes up, number of reviews also increases.

<iframe
  src="assets/bivariate_reviews_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next, we wanted to see if the number of steps of a recipe affects the rating. It appears that as the number of steps goes up, the average rating goes up as well. Possibly, people are spending more time to make better meals.

<iframe
  src="assets/bivariate_steps_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Finally, we wanted to look at the effect of season the recipe was posted on the rating. The means are pretty similar across the seasons.
<iframe
  src="assets/bivariate_by_season.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Interesting Aggregates

After having a basic understanding of the dataframe, we get this table as a summary of the relation between average ratings and the means of the different quantitative variables.

|   rating_star |   num_reviews |   n_steps |   n_ingredients |
|--------------:|--------------:|----------:|----------------:|
|             1 |       1.24262 |  10.4984  |         9.03115 |
|             2 |       1.7175  |  10.5738  |         9.17625 |
|             3 |       2.37538 |  10.0383  |         9.21574 |
|             4 |       4.28821 |   9.75441 |         9.20603 |
|             5 |       2.09116 |  10.2251  |         9.20808 |

We can see that the number of reviews has a relatively higher variance among different ratings compared to other parameters. Reviews could be an important factor to our question. There additionally appears to be a trend for the amount of sodium.

Finally, we wanted to look at the variation of reviews across the seasons the recipe was posted. It looks like those made in spring/summer have higher standard deviations than fall/winter.

In the next step, we will analyze the missing values in our dataset.

### Assessment of Missingness

There are three columns in our dataset have missing values, which are *name*, *description*, and *average_ratings*. The average ratings missingness is important to consider given our primary question.

| **Column**|**Number of Missing Values** |
| --- | --- |
| *'name'* | 1 |
| *'description'* | 70 |
| *'average_ratings '* | 2609 |

#### NMAR Analysis 

We believe that the missingness of recipe ratings could be NMAR, as if users may to only rate recipes they particularly liked or disliked. In such cases, the missingness of ratings would be related to the unobserved ratings themselves, making it NMAR.

For each quantitative in the dataframe:  
Running a permutation test to see how significant their value differs from the absence of values in *average_ratings*.

- **Null Hypothesis**: The missingness of values in *average_ratings* are not related to the value of this column.
- **Alternative Hypothesis**: The missingness of values in *average_ratings* are related to the value of this column.
- **Test Statistics**: Absolute differences in means.
- **Significance Level**: 0.05

After doing the permutation test, this is what we get:

| **Column** | **p-value** | **Relation** |
| --- | --- | --- |
| *'minutes'* | 0.042 | √ |
| *'num_reviews'* | 0.00 | √ |
| *'n_steps'* | 0.00 | √ |
| *'n_ingredients'* | 0.00 | √ |
| *'total fat'* | 0.00 | √ |
| *'sugar'* | 0.00 | √ |
| *'sodium'* | 0.87 |  |
| *'protein'* | 0.16 |  |
| *'saturated fat'* | 0.00 | √ |
| *'carbohydrates'* | 0.00 | √ |

Based on the permutation test results, we **reject the null hypothesis** for most of columns, since having a p-value less than 0.05 means that under the null hypothesis, the probability of this situation happens due to chance is less than 5%.

p-value for sugar: 0.00, reject
<iframe
  src="assets/perm_sugar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
p-value for minutes: 0.042, reject
<iframe
  src="assets/perm_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

But there are also columns not related to the missingness. For example, column *sodium* has a high p-value of **0.896**, and *protein* column has a p-value of **0.16**, which suggests that we **fail to reject** the null hypothesis of those two columns.

p-value for *sodium*: 0.87, fail to reject
<iframe
  src="assets/perm_sodium.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin-bottom: 20px;"
></iframe>
p-value for *protein*: 0.16, fail to reject
<iframe
  src="assets/perm_protein.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Related columns**: *minutes*, *num_reviews*, *n_steps*, *n_ingredients*, *total fat*, *sugar*, *saturated fat*, *carbohydrates*.

**Conclusion**: The **related columns** are related to the missing values of *average_ratings*. Or we can say, the missingness of *average_ratings* is **Missing At Random (MAR)** that related with values of related columns.

**Imputation**: After the analysis, we did probabilitic permutation based on *num_reviews*. That means the missing value of *average_ratings* will be filled with the value that has the similar *num_reviews*.  
After the imputation, the mean of *average_ratings* changed from 4.625 to 4.624 and the standard deviation changed from 0.640 to 0.645.

### Hypothesis Testing

For our hypothesis test, we wanted to examine the relationship between number of reviews and the average rating of a recipe.

**Null Hypothesis**: There is no association between number of reviews and ratings.

**Alternative Hypothesis**: There is a significant association between number of reviews and ratings. We expect the number of reviews to be positively associated with the ratings as more people may provide a review if it was a good recipe.
 
<iframe
  src="assets/hypo_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

p < 0.05. Thus, we reject the null hypothesis that number of steps is correlated to average ratings. 

### Framing a Prediction Problem

We plan to create a **regression model** using above features to predict the **average rating** of the recipe. The reason we choose average rating is because the ratings shows people's overall opinion on the recipe and we want to know which recipes are most liked. We chose R^2, root mean squared error as these metrics are appropriate for evaluating a regressor. Additionally, we will evaluate the magnitude and direction of the coefficients, to inform which factors play the largest role and in what direction.

R-squared (R^2): Represents the proportion of the variance in the dependent variable (the variable being predicted). A higher R-squared value indicates a better fit of the model to the data.

Root Mean Squared Error (RMSE): Represents the average absolute differences between the predicted and observed values. Lower RMSE values indicate better fit.

Coefficients: In a linear regression model, coefficients indicate the relationship (direction and magnitude) between the features and the outcome variable. This is important for interpretability of our regressor.

At the time of the prediction, we would NOT know the average rating. We would know the number of reviews, the number of steps, the nutritional content, the hand-written reviews, and the season the recipe was posted.

### Baseline Model

We chose the following features based on the EDA analysis, as these variables seem to be related:

Number of reviews for a given recipe (quantitative),  

The amount of sodium in recipe (quantitative)

Our model has RMSE of 0.65, and $R^2$ value of 0.0028.  

Our current model is not good. The very low $R^2$ means that the model cannot explain the variance of the dataset; it appears to be underfit and we should add more features. Additionally, the low value for the coefficients indicate the need for other features and for feature engineering. 


### Final Model

We added the following features:

"n_steps" (quantitative): This feature was added based on the results of the hypothesis test, indicating that average rating and number of steps are correlated. 

"reviews" (qualitative): The content of the worded reviews should provide information on the rating.

"season" (qualitative): The season the recipe was posted may impact the rating, as there are different seaonsal trends.

We performed the following feature engineering:

SentimentAnalyzer: We applied this to the reviews. Value of -1 indicates a negative review, 0 indicates nutral, and 1 indicates positive review.

OneHotEncoder: We applied this to the qualitative season's column.

StandardScaler: We applied this to the quantitative columns to enable to coefficients to be interpretable. 

We tested the hyperparameter "fit_intercept."

Other hyperparameters we could have tried but didn't because it would affect ability to compare coefficients included the following:

One Hot: drop = "first" or None. If we chose None, we could not interpret the coefficients due to mulitcolliniarity.

Standard Scalar: with_mean or with_std = False or True. This would again render the coefficients uninterpretable as they would not be on the same scale.

#### IMPROVED PERFORMANCE

Our final model improved it's performance, performing well on the training and test data.

Lower RMSE of 0.6102 and higher R^2 of 0.1177: Addition of useful features.

The coefficients are higher magnitude. Specifically, the Sentiment feature shows a strong positive correlation, which makes sense (more "positive" reviews should correspond to generally higher ratings).

### Fairness Analysis

For our fairness analysis, we chose two groups: recipes posted in spring/summer and recipes posted in fall/winter. These seasons bring differing kinds of recipes being posted. Thus, the performance of our model may vary based on the season the initial recipe was posted during (and thus the season it is most likely intended for). If the model is unfair, then it would be important for us to adjust the model, or do additional training, to better capture the seasonal trends.


Hypothesis Testing

- **Null Hypothesis**: Our model is fair. Its R2 for ratings given in spring/summer and fall/winter are roughly the same, and any differences are due to random chance.
- Alternative Hypothesis: Our model is unfair. Its R2 for recipes given in spring/summer is different than for fall/winter. We hypothesize a higher R2 value for spring/summer because during this time, there are often more diverse recipes (with availability of fresh produce and with more leisure time for individuals), while fall/winter may tend to focus around holiday-focused, comfort recipes (with potentially less variability). 
- Test Statistics: Root mean squared error (mrse).
- Significance Level: alpha = 0.05

# Plot the distribution of permuted differences
plt.hist(permuted_diffs, bins=30, edgecolor='k', alpha=0.7)
plt.axvline(observed_diff, color='r', linestyle='--', label='Observed difference')
plt.xlabel('Difference in RMSE')
plt.ylabel('Frequency')
plt.legend()
plt.show()

With p = 0.001, we reject our null hypothesis that our model is fair.