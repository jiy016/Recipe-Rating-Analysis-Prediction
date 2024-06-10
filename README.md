# Recipe-Rating-Analysis-Prediction


### 1. Introduction

In this project, we plan to analyze a dataset of recipes.

Our data was originally from [food.com](https://www.food.com/). We downloaded the csv files from the [link](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view) provided on the course website. 

There are two csv files in our data set:
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


We decide to merge those two dataframes based on *recipe_id* and get the overall dataframe **recipes**. Here is what it looks like:

|    | name                              |     id |   minutes |   contributor_id | submitted   | tags                              | nutrition                                    |   n_steps | steps                             | description                       | ingredients                       |   n_ingredients |          user_id |   recipe_id | date       |   rating | review                            |
|---:|:----------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------|:---------------------------------------------|----------:|:----------------------------------|:----------------------------------|:----------------------------------|----------------:|-----------------:|------------:|:-----------|---------:|:----------------------------------|
|  0 | 1 brownies in the world    bes... | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-t... | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | ['heat the oven to 350f and ar... | these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 | 386585           |      333281 | 2008-11-19 |        4 | These were pretty good, but to... |
|  1 | 1 in canada chocolate chip coo... | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-t... | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | ['pre-heat oven the 350 degree... | this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 | 424680           |      453467 | 2012-01-26 |        5 | Originally I was gonna cut the... |
|  2 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |  29782           |      306168 | 2008-12-31 |        5 | This was one of the best brocc... |
|  3 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |      1.19628e+06 |      306168 | 2009-04-13 |        5 | I made this for my son's first... |
|  4 | 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 | 768828           |      306168 | 2013-08-02 |        5 | Loved this.  Be sure to comple... |



This is our **Centered Question: which variable impact the average rating of a recipe?**

> num of rows, name of cols, why relevant




### 2. Data Cleaning and Exploratory Data Analysis

>Head of the cleaned dataframe
<iframe
  src="assets/carbohydrates_vs_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



This is another line
### 3. Assessment of Missingness
### 4. Hypothesis Testing
### 5. Framing a Prediction Problem
### 6. Baseline Model
### 7. Final Model
### 8. Fairness Analysis

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
