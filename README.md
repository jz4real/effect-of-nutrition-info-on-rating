**###effect-of-nutrition-info-on-rating**
**by: Rukun Zhang (ruz044@ucsd.edu)** 
- dsc80-final-project
- This is the analysis based on recipe information web-scraped in food.com

---
**## Introduction** 

# I used food-related and recipe-related data that has value through web scraped from food.com. Popular recipes that have accumulated throughout time, together with their general information, are included in the data. Given the size of the data, subsetting it based on a few criteria is essential, such as having more than four reviews, having more stages to cook more than four steps, and having a year that is after 2008.

---

**## Data Cleaning and Exploratory Data Analysis**

# In this case, the orginal data is consider somewhat very messy, the data columns that need to cleaned are 'tags', 'ingredients', 'nutrition', and 'steps'. These columns are either have missing values or their value types are in incorrect type compare to their autual value that need to be adjust for future using.
<iframe src="assets/pic/Rating-vs-Nutritional-Content.html" width="800" height="600" frameborder="0"></iframe>

- This is the plot with relationship between variable ['calories', 'sodium', 'total_fat', 'sugar', 'protein'] on the y variable protein

---

**## Assessment of Missingness**

# The phrase "Not Missing at Random," or NMAR, designates a missingness value that is established by itself or by additional unobserved data. Knowing the significance and defination of the column itself might be helpful in this situation. If some nutrition facts are missing from a recipe in certain circumstances, there's a high probability that the recipe itself is what determines what's missing. In this instance, food-related experience may also play an influential role because there's a potential that healthier foods have more accurate nutrition information and an updated system. Analyzing a classifier's significance, however, can also be beneficial. Understanding the main characteristics and functionality of the classifier will help us comprehend the predicted "y" variable more clearly. If a particular feature (X variable) has missing values, such as a certain X with high(or low) importance, we may examine the missing data more thoroughly by comprehending how these values affect the model's fitting and prediction.
<iframe src="assets/pic/Observed-Statistic.html" width="800" height="600" frameborder="0"></iframe>

- This is the distribution plot with permuted diffs and the observation diffs of 2 columns' missingness
- According to the analysis, the observed difference is 0.0 and the p-value is 1.0. I can get the following conclusion:there is no direct evidence to prove that there is a direct relationship bwtween 2 columns that I was testing. Furthermore, there is no direct evidence to prove the result that the missingness of one of the selected column depend on another column that were in my test.

---

**## Hypothesis Testing**
- Null Hypothesis (H0): There is no difference in the average rating between restaurants in the "low" and "high" price categories
- Alternative Hypothesis (H1): There is a significant difference in the average rating between restaurants in the "low" and "high" price categories.
- Observed Difference: 0.09879467466251635
- P-Value: 0.1847
In this case, since the p-value is larger than 0.05 at a 95% confidence level,we fail to reject the null hypothesis, means that any observed difference could be due to random variation, and we do not have enough evidence to conclude a significant difference in average ratings by price category.
<iframe src="assets/pic/Permutation-Test-Distribution-for-ColumnB.html" width="800" height="600" frameborder="0"></iframe>

- This is the distribution plot with permuted diffs and the observation diffs

---

**## Framing a Prediction Problem**
- prediction problem: How do details about a recipe, including its nutrition and components, and review, affect the recipe's overall rating and users' opinions?
- The method I am planning using is RandomForestClassifier, the variable that I am predicting is rating. The reason that I chose this column is because this varible is relatively useful by predicting its outcome compare to other columns. For the x variable, I chose ['calories', 'sodium', 'total_fat', 'sugar', 'protein'] to generate a prediction to the rating variable.
- The metric model I used to evaluate this model is Classification report. This report is specific enough so that it provided information such as Accuracy, recall, and procision with the overall model. What is more, I also added importance of each x varible as a descending order in the end. So that it clealy illustrate the weight of each x varible inside the vaiable list in gereral to determine a ratio in the predicting process.

---

**## Baseline Model**
# The model I used is call RandomForestClassifier. I use this classifier because of its overall effectiveness and adaptability to handle different kinds of data. For example, RamdomForestClassifier is able to handle datasets that contain both quantitative and qualitative data with simplicity. The recipe dataset has both numerical and string variables since it contains a variety of categorical variables and data, such as ingredients, which are saved as quantitative data, and nutrition information, which is kept as qualitative data. Compared to some of the other classifiers, RandomForestClassifier performs better with its default configuration, thus fewer unnecessary and avoidable modifications are required. Its performance may be additionally exhibited by the complexity such as processing and conversion through enormous volumes of data. Furthermore, RandomForestClassifier utilizes a multiple decision tree prediction mean, which reduces the likelihood of overfitting the trained data in comparison to models using a single decision tree.The outcome result is showing at below.

|           | precision | recall | f1-score | support |
|-----------|-----------|--------|----------|---------|
| 0         | 0.33      | 0.03   | 0.05     | 36      |
| 1         | 0.88      | 0.99   | 0.93     | 249     |
|           |           |        |          |         |
| accuracy  |           |        | 0.87     | 285     |
| macro avg | 0.60      | 0.51   | 0.49     | 285     |
| weighted avg | 0.81   | 0.87   | 0.82     | 285     |

- here is the table with all importance with all x vaiables
|           | Impotance |
|-----------|-----------|
| calories  |  0.231775 |
| sugar     |  0.200646 |
| total_fat |  0.194525 |
| protein   |  0.190267 |
| sodium    |  0.182787 |


---

**## Final Model**
# The feathers I added are Polynomial features, Standard scaler, Pipeline such as function and column transformer, and also GridSearchCV.
- Polynomial features included the possablility of non-linear relationship bwtween the variable calories and protein amount. If these features are indeed have non-linaer relationship between the predicted varible, this model can definitly improve the overall performance and accuracy by capturing more complexity cases in the behind.
- The Standard scaler case is beneficial for the model not be so sensitive to the adjustment that caused by fitting by normalize the distribution of calories's data.
- The application of Pipeline is combine engineer features with different kinds of classifier together which makes transformations in the same pace. Making sure the training set are receiving same procedures of transformations is significant during the process of predicting. What is more, this is also beneficial for the cross-validation process.
- For the GridSearchCV, this method use cross-validation to find the most precies validation set by paremeters adjustement and multiple combinations. This improve the performance of the model by improve the hyperparemeters. In this case I used a cv number to performance this feature.
- The result of this model is showing in below. I should say this model is genrally better compare to the baseline model. The recall and f1-socre amount are both increase in class 1, as well as the Precision in class 0 has also increased. Which increased both classes' predcition and their accuracy in gereral.

|           | precision | recall | f1-score | support |
|-----------|-----------|--------|----------|---------|
| 0         | 1.00      | 0.03   | 0.05     | 36      |
| 1         | 0.88      | 1.00   | 0.93     | 249     |
|           |           |        |          |         |
| accuracy  |           |        | 0.88     | 285     |
| macro avg | 0.94      | 0.51   | 0.49     | 285     |
| weighted avg | 0.89   | 0.88   | 0.82     | 285     |

---

**## Fairness Analysis**


---




