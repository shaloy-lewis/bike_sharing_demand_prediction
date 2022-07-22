<h1 align="center"> Bike Sharing Demand Prediction</h1>
<h5 align="center"> AlmaBetter Verfied Project - <a href="https://www.almabetter.com/"> AlmaBetter School </a> </h5>

<p align="center"> 
<img src="images/bike_sharing.gif" alt="..." height="175px">
</p>

![--](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 1. Abstract
* Currently Rental bikes are introduced in many urban cities for the enhancement of mobility comfort. It is important to make the rental bike available and accessible to the public at the right time as it lessens the waiting time. Eventually, providing the city with a stable supply of rental bikes becomes a major concern. The crucial part is the prediction of bike count required at each hour for the stable supply of rental bikes.
* The goal of this project is to build a ML model that is able to predict the demand of rental bikes in the city of Seoul. We are provided with the data of weather conditions, office holidays, and the number of bikes rented each hour for 12 months.

![--](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

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

![--](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 3. Feature Engineering

### a. Dew point temperature
* We know that the dew point temperature is related to temperature and can be approximated as follows:
    Td = T - ((100 - RH)/5)
    Where,
    Td - Dew point temperature (in degrees Celsius)
    T - Observed temperature (in degrees Celsius)
    RH - Relative humidity (in percent)
* The correlation between temperature and dew point temperature is 0.912798
* Hence, we can drop the column from the dataset since it will not increase the accuracy of predictions, and will only increase the model complexity.

### b. New columns from the 'date' column:
Added 3 new columns as follows:
* Month – Month of the year
* Day_of_week – 0-6 – Monday-Sunday
* Weekend – Whether a given day is a weekend (1) or not (0)

![--](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 4. EDA Summary

* The dependent variable is positively skewed.
* Normally distributed attributes: temperature, humidity.
* Positively skewed attributes: wind, solar_radiation, snowfall, rainfall.
* Negatively skewed attributes: visibility.
* Positively correlated variables: temperature, windspeed, visibility, solar radiation.
* Negatively correlated variables: humidity, rainfall, snowfall.
* The number of bikes rented is on average higher during the rush hours.
* The rented bike counts is higher during the summer and lowest during the winter.
* The rented bike count is higher on working days than on non-working days.
* On a non-functioning day, no bikes are rented in all the instances of the data.
* The number of bikes rented on average remains constant throughout Monday - Saturday, it dips on Sunday, and on average, the rented bike counts is lower on weekends than on weekdays.
* There is no multicollinearity in the data.
* Temperature has the highest correlation with the dependent variable
* There are outliers in the data and this must be taken into consideration in the model building phase.
* In winters the overall demand for rented bikes is comparatively lower than that of other seasons.
* On a non-functioning day, no bikes are rented.
* The demand for rented bikes throughout the day on holidays and weekends follow a different pattern than other days.
* On regular days, the demand for the bikes is higher during rush hours. On holidays or weekends, the demand is comparatively lower in the mornings, and is higher in the afternoons

![--](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 5. Modelling Summary
* Choice of split: K-fold cross validation, where K = 6
  This choice of split was chosen because we had the computational power available to use this split, and thereby reducing overfitting
* Evaluation metrics: RMSE – Root Mean Square Error
  During the EDA phase, we find that the data we are dealing with contains a lot of outliers. Hence to punish the outliers, we choose this metric.
* Hyperparameter tuning: GridsearchCV
  Grid Search combines a selection of hyperparameters established by the scientist and runs through all of them to evaluate the model’s performance. Through this     method, we can establish the set of parameters such that they do not overfit the data.

### a. Decision tree
* This can be considered as the baseline model to obtain predictions. Decision trees are easy to explain, and tend to have low bias and high variance (Overfitting).
* Best hyperparameters: max_depth = 20, min_samples_leaf = 30
* Decision Tree train RMSE: 224.90
* Decision Tree test RMSE: 240.64

### b. Random forests
* Random Forest is an ensemble technique which use multiple decision trees and a technique called Bootstrap and Aggregation, commonly known as bagging. The basic idea behind this is to combine multiple decision trees in determining the final output rather than relying on individual decision trees.
* Best hyperparameters: min_samples_leaf = 25, n_estimators = 500
* Random Forests train RMSE: 210.61
* Random Forests test RMSE: 238.19

### c. Gradient Boosting Method (GBM)
* In gradient boosting, each predictor corrects its predecessor’s error. Each predictor is trained using the residual errors of predecessor as labels.
* Best hyperparameters: min_samples_leaf = 26, n_estimators = 500
* GBM train RMSE: 160.93
* GBM test RMSE: 189.36

### d. Extreme Gradient Boosting (XG Boost)
* In XG Boost, decision trees are created in sequential form. Weights are assigned to all the independent variables which are then fed into the decision tree which predicts results. The weight of variables predicted wrong by the tree is increased and the variables are then fed to the second decision tree. These individual classifiers/predictors then ensemble to give a strong and more precise model.
* Best hyperparameters: min_samples_leaf = 25, n_estimators = 500
* XG Boost train RMSE: 157.58
* XG Boost test RMSE: 188.85

![--](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 6. Challenges faced
* Comprehending the problem statement
* Feature engineering – deciding on the features to be dropped/kept/transformed
* Choosing the best visualization to show the trends clearly in the EDA phase
* Deciding on ways to handle outliers
* Deciding on the ML model to make predictions
* Deciding on the best evaluation metric to evaluate the models
* Coming up with the best hyperparameters, so that the models do not overfit

![--](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 7. Conclusions
It was found that the XG Boost model had the lowest test RMSE.

The final choice of model for deployment depends on:
* If it is absolutely necessary to have a model with the best accuracy, then XG boost will be the best choice, since it has the lowest RMSE than other models built.
* But we know that, higher the model complexity, lower is the model interpretability. Hence if the predictions must be explained to stakeholders, then XG Boost is not an ideal choice.
* In this case decision tree can be used, since they are easier to explain. By choosing a simpler model, we will be compromising with the model accuracy (Accuracy vs Interpretability trade-off).
