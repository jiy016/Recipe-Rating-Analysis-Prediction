# Recipe-Rating-Analysis-Prediction


### 1. Introduction

In this project, we plan to analyze a dataset of recipes.

Our data was originally from [food.com](https://www.food.com/). We downloaded the csv files from the [link](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view) provided on the course website. 

There are two csv files in our data set:
>RAW_recipes.csv contains recipes.
>RAW_interactions.csv contains reviews and ratings submitted for the recipes in RAW_recipes.csv.

for recipes dataframe, it is 



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
