# effect-of-nutrition-info-on-rating

## **by: Rukun Zhang (ruz044@ucsd.edu)** 
- dsc80-final-project
- This is the analysis based on recipe information web-scraped in food.com

---
## Introduction

### I used food-related and recipe-related data that has value through web scraped from food.com. Popular recipes that have accumulated throughout time, together with their general information, are included in the data. Given the size of the data, subsetting it based on a few criteria is essential, such as having more than four reviews, having more stages to cook more than four steps, and having a year that is after 2008.

---

## Data Cleaning and Exploratory Data Analysis

### In this instance, the original data is thought to be somewhat disorganized; the "tags," "ingredients," "nutrition," and "steps" data columns need to be cleaned. These columns need to be adjusted for future use because they either contain missing values or have value types that are different from their actual values. PS: since I am also calculating the tf-idf of all meaningful words inside the the review column to find the most meaningful words and their score, I am sampling the original dataframe with 500 samples in order to avoid a large runtime.
- Below is an updated version of the cleaned dataframe that excludes the 'review' column, which has a significant amount of text space, and includes features of the to-certain available value for future purposes.
 
|   calories |   total_fat |   sugar |   sodium |   protein |   rating |   n_ingredients |
|------------|-------------|---------|----------|-----------|----------|-----------------|
|      123.3 |           2 |       4 |       12 |         7 |        5 |               7 |
|      161.9 |           3 |      80 |        9 |         5 |        5 |               3 |
|      377.3 |          11 |     200 |       25 |         7 |      nan |               9 |
|      377.3 |          11 |     200 |       25 |         7 |        5 |               9 |
|      546.1 |          48 |     172 |       15 |        12 |        4 |              12 |

<iframe src="asset/pic/Distribution-of-rating.html" width="800" height="600" frameborder="0"></iframe>

- The rating for names in a single column is distributed as follows. Based on the distribution, it appears that ratings occur in whole numbers and integers, such 3, 4, and 5. Additionally, there appears to be an imbalance between high and low ratings. Compared to all other ratings combined, there are significantly more ratings of 5.

<iframe src="asset/pic/Rating-vs-Nutritional-Content.html" width="800" height="600" frameborder="0"></iframe>

- This figure illustrates the connection between the variables ['calories', 'sodium', 'total_fat, 'sugar', 'protein,'] on the rating of the y variable. The figure indicates that there appears to be a positive trend with the dependent variable y (rating) as each independent variable increases in amount. 

---

## Assessment of Missingness

### "Not Missing at Random," or NMAR, is a designation for a missingness value that can be determined by extraneous data or by the data itself. In this case, understanding the meaning and definition of the column itself may be useful. In the scenario that some nutrition information is omitted from a recipe, it is quite likely that the recipe specifies what is missing. In this case, food-related expertise could also be significant since it's possible that updated systems and more precise nutrition information are available for healthier recipes. Nonetheless, examining the importance of a classifier can also be helpful. We can better understand the predicted "y" variable if we are aware of the classifier's primary features and operational mechanisms. If a certain feature (X variable), such as a certain X with high(or low) relevance, has missing values, we may investigate the missing data in further detail by understanding how these values impact the fitting and prediction of the model.
<iframe src="asset/pic/Observed-Statistic.html" width="800" height="600" frameborder="0"></iframe>

- This is the distribution plot of the two columns' missingness using permuted diffs and observation diffs.
- The analysis indicates that the p-value is 1.0 and the observed difference is 0.0. I've come to the following conclusion: there isn't any concrete proof and tangible evidence that the two columns I was examining have an obvious connection. Moreover, there is insufficient evidence to support the claim that the absence of a certain column in my test depends on the presence of another column.

---

## Hypothesis Testing
- Null Hypothesis (H0): There is no difference in the average rating between restaurants in the "low" and "high" price categories
- Alternative Hypothesis (H1): There is a significant difference in the average rating between restaurants in the "low" and "high" price categories.
- Observed Difference: 0.09879467466251635
- P-Value: 0.1847
We are unable to reject the null hypothesis in this instance since the p-value is greater than 0.05 at a 95% confidence level. This suggests that any observed difference may be the result of random fluctuation, and we lack sufficient data to draw the conclusion that there is a significant difference in average ratings per price category.
<iframe src="asset/pic/Permutation-Test-Distribution-for-ColumnB.html" width="800" height="600" frameborder="0"></iframe>

- This is the distribution plot with permuted diffs and the observation diffs

---

## Framing a Prediction Problem
- prediction problem: How do details about a recipe, including its nutrition and components, and review, affect the recipe's overall rating and users' opinions?
- I want to use RandomForestClassifier as my approach, and rating is the variable I'm forecasting. I selected this particular column because, in comparison to other columns, it predicts an outcome that is comparatively beneficial. To create a forecast for the rating variable, I selected ['calories,' 'sodium', 'total_fat', 'sugar', 'protein'] for the x variable.
- I assessed this model using the Classification report matrix model. Because of its specificity, the study included details on the accuracy, recall, and precision of the entire model. Furthermore, I included each x variable's relevance in descending order at the conclusion. In order to establish a ratio throughout the prediction process, it must clearly display the weight of each x variable inside the variable list in general.

---

## Baseline Model
### I used a model known as RandomForestClassifier. I employ this classifier due to its general effectiveness and adaptability in handling various types of data. For instance, datasets comprising both quantitative and qualitative data may be easily handled by RamdomForestClassifier. Since the recipe dataset includes a range of categorical variables and data—such as ingredients, which are stored as quantitative data, and nutrition information, which is retained as qualitative data—it comprises both numerical and string variables. RandomForestClassifier performs better with its default setup than some of the other classifiers, therefore needless and wasteful tweaks are needed. More complexity, such as processing and converting vast amounts of data, might demonstrate its performance. Additionally, compared to models that use a single decision tree, RandomForestClassifier's use of a multiple decision tree prediction lessens the possibility of overfitting the trained data. The final result is displayed below.

|           | precision | recall | f1-score | support |
|-----------|-----------|--------|----------|---------|
| 0         | 0.33      | 0.03   | 0.05     | 36      |
| 1         | 0.88      | 0.99   | 0.93     | 249     |
|           |           |        |          |         |
| accuracy  |           |        | 0.87     | 285     |
| macro avg | 0.60      | 0.51   | 0.49     | 285     |
| weighted avg | 0.81   | 0.87   | 0.82     | 285     |

- here is the table with all importance with all x variables
|           | Importance |
|-----------|-----------|
| calories  |  0.231775 |
| sugar     |  0.200646 |
| total_fat |  0.194525 |
| protein   |  0.190267 |
| sodium    |  0.182787 |


---

## Final Model
### The feathers I added are Polynomial features, Standard scaler, Pipeline such as function and column transformer, and also GridSearchCV.
- One of the characteristics of polynomials was the potential for a non-linear relationship between the variable quantity of protein and calories. By incorporating more complicated situations in the back, this model can undoubtedly increase overall performance and accuracy if these characteristics do, in fact, have a non-linear connection with the predicted variable.
- By normalizing the distribution of the calories data, the Standard Scaler Case helps the model become less sensitive to fitting adjustments.
- Pipeline is used to apply several classifier types and engineer features together to create transformations at the same rate. During the prediction phase, it is important to ensure that the training set is undergoing the same transformation methods. Additionally, this helps with the cross-validation procedure.
- Cross-validation is used in the GridSearchCV technique to determine the most precise validation set by adjustment of parameters and various combinations. By improving the hyperparameters, this enhances the model's performance. In this instance, I performed this functionality using a CV number of 6.
- The result of this model is shown below. I should say this model is generally better compared to the baseline model. The recall and f1-score amounts are both increase in class 1, as well as the Precision in class 0 has also increased. Which increased both classes' predictions and their accuracy in general.

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

- In this instance, my group of 'x' is the number of components (the n_ingredients column), which is fewer than the column values' median, which is around 14. In contrast, my group of 'y' is the column value that is more than the column value's median. This allowed me to use Binarizer to build a binary value, and I set the threshold at 14 as the median value of the n_ingredients column.
- The assessment measure I employed is a Python function called precision score, which I imported. It computes the score between groups X and Y_pred and Group Y and Y_pred and displays their outcomes. 
- **Null Hypothesis**: The model is fair. Its precision for less number of ingredients and more number of ingredients are roughly the same, and any differences are due to random chance.
- **Alternative Hypothesis**: The model is unfair. Its precision for less number of ingredients is lower than its precision for more number of ingredients.
- I set the significance threshold at 95% and utilized the p-value as my test statistic. I ultimately obtained a p-value of 0.387, which is more than the significance limit of 0.05. In this instance, the conclusion shows that we should accept the null hypothesis, according to which the model is reasonably fair and the observed difference is reasonably likely to occur by random chance.

Below is a visualization of that related to the new permutation test that is related to the number of infregient separated into 2 classes which is bigger than the median of 14 and another is smaller than 14.
<iframe src="asset/pic/Permutation-Test-Distribution-with-Groups-of-n_ingredients.html" width="800" height="600" frameborder="0"></iframe>

---




