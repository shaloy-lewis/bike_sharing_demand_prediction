# Bike Sharing Demand Prediction

## 1. Abstract
* Currently Rental bikes are introduced in many urban cities for the enhancement of mobility comfort. It is important to make the rental bike available and accessible to the public at the right time as it lessens the waiting time. Eventually, providing the city with a stable supply of rental bikes becomes a major concern. The crucial part is the prediction of bike count required at each hour for the stable supply of rental bikes.
* The goal of this project is to build a ML model that is able to predict the demand of rental bikes in the city of Seoul. We are provided with the data of weather conditions, office holidays, and the number of bikes rented each hour for 12 months.

## 2. Introduction
* The dataset contains weather information (Temperature, Humidity, Windspeed, Visibility, Dewpoint, Solar radiation, Snowfall, Rainfall), the number of bikes rented per hour and date information.

### Attribute Information:
* Date - year-month-day
* Rented Bike count - Count of bikes rented at each hour
* Hour - Hour of the day
* Temperature - Celsius
* Humidity - %
* Windspeed - m/s
* Visibility - 10m
* Dew point temperature - Celsius
* Solar radiation - MJ/m2
* Rainfall - mm
* Snowfall - cm
* Seasons - Winter, Spring, Summer, Autumn
* Holiday - Holiday/No holiday
* Functional Day - NoFunc(Non Functional Hours), Fun(Functional hours)

## 3. Feature Engineering

### a. Dew point temperature
* We know that the dew point temperature is related to temperature and can be approximated as follows:
    Td = T - ((100 - RH)/5)
    Where,
    Td - Dew point temperature (in degrees Celsius)
    T - Observed temperature (in degrees Celsius)
    RH - Relative humidity (in percent)
* The correlation between temperature and dew point temperature is 0.912798
![image](https://user-images.githubusercontent.com/61943716/177028439-d25a3050-4c5b-4d73-b768-f18009f407cc.png)
* Also, from the above scatter plot, it is clear that as the temperature increases, the dew point temperature also increases. Hence, we can drop the column from the dataset since it will not increase the accuracy of predictions, and will only increase the model complexity.

### b. New columns from the 'date' column:
Added 3 new columns as follows:
* Month – Month of the year
* Day_of_week – 0-6 – Monday-Sunday
* Weekend – Whether a given day is a weekend (1) or not (0)

## 4. Exploratory Data Analysis

### Analyzing the distribution of the dependent variable:
![image](https://user-images.githubusercontent.com/61943716/177028528-9cf0eb5a-c52c-408e-b5e7-c6bcf526b5d7.png)
* The dependent variable is positively skewed.

### Analyzing the distribution of the continuous independent variables:
![image](https://user-images.githubusercontent.com/61943716/177028572-c66caa3e-98eb-40fd-85a4-bcf2df593ceb.png)
![image](https://user-images.githubusercontent.com/61943716/177028575-d9407de4-86b4-431a-9059-0482c112eb72.png)
![image](https://user-images.githubusercontent.com/61943716/177028578-d26ff151-c657-4c49-b3d7-dbdaba15aa1a.png)
![image](https://user-images.githubusercontent.com/61943716/177028584-510da2fc-d3b2-402c-9b69-f56c91a2a08f.png)
![image](https://user-images.githubusercontent.com/61943716/177028587-2be08585-99d6-48fc-bb27-4c235ff83cc7.png)
![image](https://user-images.githubusercontent.com/61943716/177028591-6670f344-5d24-4645-aab1-697f273aa048.png)
![image](https://user-images.githubusercontent.com/61943716/177028596-47310774-ca6a-4b4f-aba5-963141358260.png)
* Normally distributed attributes: temperature, humidity.
* Positively skewed attributes: wind, solar_radiation, snowfall, rainfall.
* Negatively skewed attributes: visibility.

### Analyzing the relationship between the dependent variable and the continuous variables in the data:
![image](https://user-images.githubusercontent.com/61943716/177028633-e63fc0eb-8605-485b-86cc-87de9351b39d.png)
![image](https://user-images.githubusercontent.com/61943716/177028636-5f2f33b6-5009-4352-8d1b-32a08fc75984.png)
![image](https://user-images.githubusercontent.com/61943716/177028652-55923395-0cd7-4ddf-abc9-f9981831239e.png)
![image](https://user-images.githubusercontent.com/61943716/177028657-b5b7e266-5e77-4a53-92c3-e974009789da.png)
![image](https://user-images.githubusercontent.com/61943716/177028665-2d557907-fb1e-4122-ab36-c7d252cd6405.png)
![image](https://user-images.githubusercontent.com/61943716/177028646-9a0d2153-7c72-4285-bb8c-c7e5e96a0006.png)
![image](https://user-images.githubusercontent.com/61943716/177028700-69a2affb-7661-4f5c-9ef8-a24a25657774.png)
* Positively correlated variables: temperature, windspeed, visibility, solar radiation.
* Negatively correlated variables: humidity, rainfall, snowfall.

### Analyzing the relationship between the dependent variable and the categorical variables in the data:
![image](https://user-images.githubusercontent.com/61943716/177028704-21cc1f19-e5cf-4011-be9a-1046accb4f24.png)
![image](https://user-images.githubusercontent.com/61943716/177028706-29abee32-6516-4a09-a08f-17608452e334.png)
![image](https://user-images.githubusercontent.com/61943716/177028699-e9a0a4c2-9db8-47db-8135-a05e84874ad7.png)
![image](https://user-images.githubusercontent.com/61943716/177028710-7b97c40c-a254-45f9-bbdc-c30c670fba38.png)
![image](https://user-images.githubusercontent.com/61943716/177028714-50ebae92-6272-4837-80a8-02c5114ea730.png)
![image](https://user-images.githubusercontent.com/61943716/177028715-f7e1a837-66e0-4b93-bc69-27829875b2c6.png)
* The number of bikes rented is on average higher during the rush hours.
* The rented bike counts is higher during the summer and lowest during the winter.
* The rented bike count is higher on working days than on non-working days.
* On a non-functioning day, no bikes are rented in all the instances of the data.
* The number of bikes rented on average remains constant throughout Monday - Saturday, it dips on Sunday, and on average, the rented bike counts is lower on weekends than on weekdays.

### Correlation analysis:
![image](https://user-images.githubusercontent.com/61943716/177028772-4235336f-7596-42f1-84a4-53d31c273672.png)
* There is no multicollinearity in the data.
* Temperature has the highest correlation with the dependent variable

### Outlier analysis:
![image](https://user-images.githubusercontent.com/61943716/177028770-235943a8-8926-4d81-8568-4cb08e3a8d05.png)
![image](https://user-images.githubusercontent.com/61943716/177028773-9ca06533-aa56-4f7b-baca-521cc7b3e5f0.png)
![image](https://user-images.githubusercontent.com/61943716/177028774-415dd31b-d4c3-46ee-bfc5-3c9e6d338bf6.png)
![image](https://user-images.githubusercontent.com/61943716/177028776-f5fad6ba-9820-46d8-8f0c-1864a01257b3.png)
![image](https://user-images.githubusercontent.com/61943716/177028771-80aab8ac-acc6-4567-ab7c-44dc860a8468.png)
![image](https://user-images.githubusercontent.com/61943716/177028735-8d35c0a1-3b89-4d13-89bc-1626f8d4d892.png)
![image](https://user-images.githubusercontent.com/61943716/177028848-23c56c31-0252-4bcf-8118-0c81f0daa274.png)
* There are outliers in the data and this must be taken into consideration in the model building phase.

### Bike demand throughout the day:
![image](https://user-images.githubusercontent.com/61943716/177028851-9c21c9b1-28b3-4b8b-b1aa-396102ccdc16.png)
![image](https://user-images.githubusercontent.com/61943716/177028867-a513f140-1cbc-42d7-8334-8c4e6036eb93.png)
![image](https://user-images.githubusercontent.com/61943716/177028868-ba2be165-e65d-45c1-a1f5-e74ef45d71a3.png)
![image](https://user-images.githubusercontent.com/61943716/177028869-353cc824-c050-491b-a682-102996814a79.png)
![image](https://user-images.githubusercontent.com/61943716/177028866-18deb9ce-6556-4931-bade-b34f7daae915.png)
![image](https://user-images.githubusercontent.com/61943716/177028642-6480237c-1c20-45b8-89fa-d6e7a80a61a7.png)
* In winters the overall demand for rented bikes is comparatively lower than that of other seasons.
* On a non-functioning day, no bikes are rented.
* The demand for rented bikes throughout the day on holidays and weekends follow a different pattern than other days.
* On regular days, the demand for the bikes is higher during rush hours. On holidays or weekends, the demand is comparatively lower in the mornings, and is higher in the afternoons

## 5. Modeling
* Choice of split: K-fold cross validation, where K = 6
  This choice of split was chosen because we had the computational power available to use this split, and thereby reducing overfitting
* Evaluation metrics: RMSE – Root Mean Square Error
  During the EDA phase, we find that the data we are dealing with contains a lot of outliers. Hence to punish the outliers, we choose this metric.
* Hyperparameter tuning: GridsearchCV
  Grid Search combines a selection of hyperparameters established by the scientist and runs through all of them to evaluate the model’s performance. Through this     method, we can establish the set of parameters such that they do not overfit the data.

### a. Decision tree
![image](https://user-images.githubusercontent.com/61943716/177029074-58e16148-6bfd-4268-8866-c9b037daaa3b.png)
* This can be considered as the baseline model to obtain predictions. Decision trees are easy to explain, and tend to have low bias and high variance (Overfitting).
* Best hyperparameters: max_depth = 20, min_samples_leaf = 30
* Decision Tree train RMSE: 224.90266499660783
* Decision Tree test RMSE: 240.64140324429667
![image](https://user-images.githubusercontent.com/61943716/177029085-f43b4a55-d8a3-4563-a7ec-9c5091ffe12c.png)

### b. Random forests
![image](https://user-images.githubusercontent.com/61943716/177029092-886c2816-e2dd-4c76-9e20-06badda47422.png)
* Random Forest is an ensemble technique which use multiple decision trees and a technique called Bootstrap and Aggregation, commonly known as bagging. The basic idea behind this is to combine multiple decision trees in determining the final output rather than relying on individual decision trees.
* Best hyperparameters: min_samples_leaf = 25, n_estimators = 500
* Random Forests train RMSE: 210.6173065025053
* Random Forests test RMSE: 238.19319904214672
![image](https://user-images.githubusercontent.com/61943716/177029130-98ad3538-9505-4fe0-9c44-4f858c9fd653.png)

### c. Gradient Boosting Method (GBM)
![image](https://user-images.githubusercontent.com/61943716/177029145-70580816-9b58-495e-ab7b-c1c0ab3db8cb.png)
* In gradient boosting, each predictor corrects its predecessor’s error. Each predictor is trained using the residual errors of predecessor as labels.
* Best hyperparameters: min_samples_leaf = 26, n_estimators = 500
* GBM train RMSE: 160.93596141805804
* GBM test RMSE: 189.36563686010564
![image](https://user-images.githubusercontent.com/61943716/177029162-79efd7b5-21d5-46a3-82b4-f2164b0b9e28.png)

### d. Extreme Gradient Boosting (XG Boost)
![image](https://user-images.githubusercontent.com/61943716/177029186-fafbe15a-95a6-4ef8-b0e4-e4c98e2ebb31.png)
* In XG Boost, decision trees are created in sequential form. Weights are assigned to all the independent variables which are then fed into the decision tree which predicts results. The weight of variables predicted wrong by the tree is increased and the variables are then fed to the second decision tree. These individual classifiers/predictors then ensemble to give a strong and more precise model.
* Best hyperparameters: min_samples_leaf = 25, n_estimators = 500
* XG Boost train RMSE: 157.58047715624056
* XG Boost test RMSE: 188.85422028824942
![image](https://user-images.githubusercontent.com/61943716/177029206-66e6a611-ddb8-4c17-8fec-a983efdf9026.png)

## 6. Model comparison
![image](https://user-images.githubusercontent.com/61943716/177029218-2ce6362f-d954-4e44-9686-eed647c50ff1.png)
* The XG boost model has the lowest train and test RMSE.

## 7. Challenges faced
* Comprehending the problem statement
* Feature engineering – deciding on the features to be dropped/kept/transformed
* Choosing the best visualization to show the trends clearly in the EDA phase
* Deciding on ways to handle outliers
* Deciding on the ML model to make predictions
* Deciding on the best evaluation metric to evaluate the models
* Coming up with the best hyperparameters, so that the models do not overfit

## 8. Conclusions
During the course of this project, we:
* Define the problem statement
* Loaded the data into Colab IDE
* Cleaned the data, and performed feature engineering
* Performed exploratory data analysis (EDA)
* Built 4 regression models to predict the dependent variable
* Evaluated the models using RMSE

It was also found that the XG Boost model had the lowest test RMSE.

The final choice of model for deployment depends on:
* If it is absolutely necessary to have a model with the best accuracy, then XG boost will be the best choice, since it has the lowest RMSE than other models built.
* But we know that, higher the model complexity, lower is the model interpretability. Hence if the predictions must be explained to stakeholders, then XG Boost is not an ideal choice.
* In this case decision tree can be used, since they are easier to explain. By choosing a simpler model, we will be compromising with the model accuracy (Accuracy vs Interpretability trade-off).

## 9. References
* GeekforGeeks
* Towards data science
* Analytics Vidhya
* ProjectPro
* Kaggle
* W3 schools
* Pythonguides
* Stackoverflow
* Python libraries technical documentation
* Tutorialspoint
