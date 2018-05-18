# Predicting Health Problems with Nutritional Status

## Abstract

Using data from National Health and Nutrition Examination Survey, this study conducted linear regression to fit model for blood pressure and used multiple techniques including logistic regression, feature selection, CatBoost, XGBoost, LightGBM and Deep Neutral Network to fit models for diabetes. Linear regression using L1 regularization(Lasso) selected weight as the explanatory variable for blood pressure and gave best prediction, comparing with L2 regularization (Ridge) and Elastic Net methods. As for diabetes, CatBoost won the single model performance. Feature selection didn’t improve the model’s performance, but feature engineering did improve the performance. CatBoost with CNN feature engineering gave the best prediction.

## Dataset and Variables

Downloaded seven datasets from NHANES website . Data from 2001 to 2014 were used in this project.  Alcohol dataset includes alcohol consuming information. The blood pressure dataset includes four measurements of systolic blood pressure and four measurements of diastolic blood pressure. Body measurements dataset contains variables: height, weight, and BMI. The demographic dataset contains variable: age, gender, education level, and marital status. The two nutrition datasets contain participants’ self-reported information about the amount of protein, fiber, carbohydrate, vitamins, water and other necessary nutrition that were taken during the two sampling day. The cardiovascular health dataset includes information about participants cardiovascular health.

## Model

### _CatBoost_

One advantage of CatBoost is that the author makes a hypothesis of the difference between the actual sampled sample and the real sample, and believes that their feature distribution is not completely consistent. This kind of assumption is in fact very consistent with the actual situation encountered, especially if the sample size is not sufficient. Catboost is more stable on small sample sets than LightGBM and XGBoost. After filling NA with median, we train 400 iterations, and extracted feature scores from it since it is a tree-based model.

### Feature Engineering with CNN

1. Pick 18 of the most important features, concatenate them into a (sample size, 36) matrix.
2. Concatenate this matrix with the first full-connected layer in DNN. So far, we obtain a (sample size, 784) matrix.
3. Reshape the features as a (28 * 28) “image” for each sample.
4. Input (sample size, 1, 28, 28) to LeNet Network
5. After training, we output the feature map of conv_1 layer. 
6. Since the important features generated by CatBoost lie in the first 36 columns, the information will be stored within columns[0, 20] after a max-pooling layers.
7. Add these 20 features into CatBoost Model, then train. 

## Outcome

Using Youdan’s Index to find optimal cut-off point in our best model (CatBoost with convolutional layer features), we obtain the threshold is 0.17. The sensitivity is 0.7355, and the specificity is 0.6583.