# Recipe-Rating-Analysis-Prediction
This project is part of DSC80 course at UCSD, which is investigating the influence of different factors on the average rating of recipes.

Authors: Lora Bednarek & Jiaxin Yang

### Introduction

An introduction and why this question is relevant.

This is our **Centered Question: which variable impact the average rating of a recipe?**


In this project, we plan to analyze a dataset of recipes.

Our data was originally from [food.com](https://www.food.com/). We downloaded the csv files from the [link](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view) provided on the course website. 

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



We will do the data cleaning and organizing in the next step.



> why relevant




### Data Cleaning and Exploratory Data Analysis

First, we decide to merge those two dataframes based on *recipe_id* and get an combined dataframe. This is what it looks like:


<div style="overflow-x:auto;">
  
|    | name                              |     id |   minutes |   contributor_id | submitted   | tags                              | nutrition                                    |   n_steps | steps                             | description                       | ingredients                       |   n_ingredients |          user_id |   recipe_id | date       |   rating | review                            |
|---:|:----------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------|:---------------------------------------------|----------:|:----------------------------------|:----------------------------------|:----------------------------------|----------------:|-----------------:|------------:|:-----------|---------:|:----------------------------------|
|  0 | 1 brownies in the world    bes... | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-t... | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | ['heat the oven to 350f and ar... | these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 | 386585           |      333281 | 2008-11-19 |        4 | These were pretty good, but to... |
|  1 | 1 in canada chocolate chip coo... | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-t... | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | ['pre-heat oven the 350 degree... | this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 | 424680           |      453467 | 2012-01-26 |        5 | Originally I was gonna cut the... |
|  2 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |  29782           |      306168 | 2008-12-31 |        5 | This was one of the best brocc... |
|  3 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |      1.19628e+06 |      306168 | 2009-04-13 |        5 | I made this for my son's first... |
|  4 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 | 768828           |      306168 | 2013-08-02 |        5 | Loved this.  Be sure to comple... |

</div>




- According to the rubric of the project, adding a new column (*rating*) of average ratings that all 0 ratings are replaced with NaN.
- Adding two columns, one contains the number of reviews for each recipe (*num_reviews*), and the other one contains the every reviews for each recipe (*reviews*). 
- Onehot encoding the *nutrition* column into 7 more columns, each containing the amount of corresponding nutrition, then drop the original *nutrition* column.
- Adding the *season* column by the *sumbitted* time. 
  - Spring: 3-5. Summer: 6-8. Fall: 7-9. Winter: 12-2.


After adding the following columns:

| **Column**|**Description** |
| --- | --- |
| *'num_reviews'* | The number of reviews of this recipe |
| *'reviews'* | Reviews of this recipe |
| *'calories '* | The amount of calories |
| *'total fat'* | The amount of total fat |
| *'sugar '* | The amount of sugar |
| *'sodium  '* | The amount of sodium |
| *'protein  '* | The amount of protein |
| *'saturated fat '* | The amount of saturated fat |
| *'carbohydrates  '* | The amount of carbohydrates |
| *'season  '* | The season of the submittion time |


This is the dataframe after the data cleaning:

|    | name                                 |     id |   minutes |   contributor_id | submitted   | tags                              |   n_steps | steps                             | description                       | ingredients                       |   n_ingredients |   average_ratings |   num_reviews | reviews                           |   calories |   total fat |   sugar |   sodium |   protein |   saturated fat |   carbohydrates | season   |
|---:|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------|----------:|:----------------------------------|:----------------------------------|:----------------------------------|----------------:|------------------:|--------------:|:----------------------------------|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|:---------|
|  0 | 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-t... |        10 | ['heat the oven to 350f and ar... | these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 |                 4 |             1 | These were pretty good, but to... |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 | fall     |
|  1 | 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-t... |        12 | ['pre-heat oven the 350 degree... | this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 |                 5 |             1 | Originally I was gonna cut the... |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 | spring   |
|  2 | 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |                 5 |             4 | This was one of the best brocc... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | spring   |
|  3 | millionaire pound cake               | 286009 |       120 |           461724 | 2008-02-12  | ['time-to-make', 'course', 'cu... |         7 | ['freheat the oven to 300 degr... | why a millionaire pound cake? ... | ['butter', 'sugar', 'eggs', 'a... |               7 |                 5 |             1 | don't let the calories and fat... |      878.3 |          63 |     326 |       13 |        20 |             123 |              39 | winter   |
|  4 | 2000 meatloaf                        | 475785 |        90 |          2202916 | 2012-03-06  | ['time-to-make', 'course', 'ma... |        17 | ['pan fry bacon , and set asid... | ready, set, cook! special edit... | ['meatloaf mixture', 'unsmoked... |              13 |                 5 |             2 | Delicious!!!!! -- the goat che... |      267   |          30 |      12 |       12 |        29 |              48 |               2 | spring   |

83782 rows × 22 columns

This **recipe** data frame is easier to process. We will use this dataframe to do the further works.

#### Univariate Analysis







>Head of the cleaned dataframe
<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


#### Bivariate Analysis

> Bivariate graphs, need explaination
<iframe
  src="assets/bivariate_carbo_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/bivariate_reviews_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/bivariate_steps_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/bivariate_by_season.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



After having a basic understanding of the dataframe, we get this table as a summary of the relation between average ratings and different variables.

|   rating_star |   num_reviews |   n_steps |   n_ingredients |
|--------------:|--------------:|----------:|----------------:|
|             1 |       1.24262 |  10.4984  |         9.03115 |
|             2 |       1.7175  |  10.5738  |         9.17625 |
|             3 |       2.37538 |  10.0383  |         9.21574 |
|             4 |       4.28821 |   9.75441 |         9.20603 |
|             5 |       2.09116 |  10.2251  |         9.20808 |

We can see that the number of reviews has a relatively higher variance among different ratings compared to other parameters. Reviews could be an important factor to our question.

In the next step, we will analyze the missing values in our dataset.


### Assessment of Missingness

There are three columns in our dataset have missing values, which are *name*, *description*, and *average_ratings*.

| **Column**|**Number of Missing Values** |
| --- | --- |
| *'name'* | 1 |
| *'description'* | 70 |
| *'average_ratings '* | 2609 |

We assume the missingness 

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
<iframe
  src="assets/perm_sugar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
p-value for sugar: 0.00, reject
<iframe
  src="assets/perm_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
p-value for mintues: 0.042, reject

But there are also columns related to the missing so much. For example, column *sodium* has a high p-value of **0.896**, and *protein* column has a p-value of **0.16**, which suggests that we **fail to reject** the null hypothesis of those two columns.
<iframe
  src="assets/perm_sodium.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin-bottom: 20px;"
></iframe>
p-value for sodium: 0.87, fail to reject
<iframe
  src="assets/perm_protein.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
p-value for protein: 0.16, fail to reject

**Related columns**: *minutes*, *num_reviews*, *n_steps*, *n_ingredients*, *total fat*, *sugar*, *saturated fat*, *carbohydrates*.

**Conclusion**: The **related columns** are related to the missing values of *average_ratings*. Or we can say, the missingness of *average_ratings* is **Missing At Random (MAR)** that related with values of related columns.


**Imputation**: After the analysis, we did probabilitic permutation based on *num_reviews*. That means the missing value of *average_ratings* will be filled with the value that has the similar *num_reviews*.  
After the imputation, the mean of *average_ratings* changed from 4.625 to 4.624.

### Hypothesis Testing

> More explaination here

**Null Hypothesis**: There is no association between number of reviews and ratings.
**Alternative Hypothesis**: There is a significant association between number of reviews and ratings.
 
<iframe
  src="assets/hypo_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

p < 0.05. Thus, we reject the null hypothesis that number of steps is correlated to average ratings. 

### Framing a Prediction Problem

> Explaination, add metric

Report:  
We focused on some quantitative features in our dataset, which are:   
Number of steps in recipe (n_steps),  
The number of ingredients of the recipe (n_ingredients),  
The amount of fat in recipe (total fat),
The amount of protein in recipe (protein),  
and The amount of carbohydrates in recipe (carbohydrates).  

We plan to create a **regression model** using above features to predict the **average rating** of the recipe. The reason we choose average rating is because the ratings shows people's ovarall preference of the compositions of the recipe, and 

(Metric, eg: F1 or accuracy)

### Baseline Model


### Final Model


### Fairness Analysis

In the fairness test part, we want to see if our model have a fair performance through [], so we use **Group X as [earlier recipes]** and **Group Y as [later recipes]**. 
On our defination, the boundary between earlier and later would be [] since it is the mean of two middle quartiles representing the average division for most recipes.
Because we are using a linear regression as our model, we decide to use rmse as our test statistic. 

- Null Hypothesis: Our linear regression model is fair. Its rmse for [earlier recipes] and []  are roughly the same, and any differences are due to chance.
- Alternative Hypothesis: Our linear regression model is not fair. Its rmse for [earlier recipes] is []er than its rmse for [].
- Test Statistics: Root mean squared error (mrse).
- Significance Level: alpha = 0.05

First, we should binarize the columns of *submitted* column in the dataframe. Adding one extra columns to the recipies called **if_earlier**






Part 8 permutation part coding:
def rmse(season):
    X_season = X[X['season'] == season]
    y_season = y[X['season'] == season]
    y_pred_season = model.predict(X_season)
    return np.sqrt(mean_squared_error(y_season, y_pred_season))

rmse_spring_summer = rmse('spring/summer')
rmse_fall_winter = rmse('fall/winter')
observed = rmse_spring_summer - rmse_fall_winter

n_permutation = 1000
season_perm_diffs = np.array([])
for i in range(n_permutation):
    shuffled_season = np.random.permutation(recipes['season'])
    np.append(rmse('sprint/summer') - rmse('fall_winter'), season_perm_diffs)

p_value = np.mean(np.abs(season_perm_diffs) >= np.abs(observed))
print(f"Observed RMSE difference: {observed_diff}")
print(f"P-value: {p_value}")

# Plot the distribution of permuted differences
plt.hist(permuted_diffs, bins=30, edgecolor='k', alpha=0.7)
plt.axvline(observed_diff, color='r', linestyle='--', label='Observed difference')
plt.xlabel('Difference in RMSE')
plt.ylabel('Frequency')
plt.legend()
plt.show()
