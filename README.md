# Seoul-Bike-Sharing-Demand-Prediction

![arc-bike2](https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/c8731a0a-795d-4a68-8e13-9039a9adbb50)

# Data Analysis

Finding details from data:

1) There are 14 features with 8760 rows of data. <br>
2) There are 4 categorical columns and 10 numerical columns. Columns ‘Date’, ‘Seasons’ and ‘Functioning Day’ are of 𝑜𝑏𝑗𝑒𝑐𝑡 data type. <br>
3) Columns ‘Rented Bike Count’, ‘Hour’, ‘Humidity (%)' and ‘Visibility (10𝑚)' are of 𝑖𝑛𝑡64 numerical data type. <br>
4) Columns ‘Temperature Temperature (℃)’, ‘Wind Speed (𝑚/𝑠)’, ‘Dew Point Temperature (℃)’,‘Solar Radiation (𝑀𝐽/𝑚2)’,‘Rainfall (𝑚𝑚)' and ‘Snowfall(𝑐𝑚) are of 𝑓𝑙𝑜𝑎𝑡64 numarical data type. <br>
5) Not any null value present in any column.<br>
6) Unique count : Seasons- 4 , Holiday- 2 , Functioning Day- 2


# Data Viz.

<img width="713" alt="__results___18_0" src="https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/e8f2b4c3-332d-4aba-85cb-91fbff0ab7d0">

![__results___29_0](https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/c99090f2-d58b-43dc-a6d3-c36f03a99605)


![__results___29_1](https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/8582566e-4e93-403d-8403-6d8934ec9d46)


![__results___29_2](https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/8d9f994f-be12-4344-a173-c5093ae907d1)



![__results___29_5](https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/a4c6d40f-3462-419a-8175-44ebaa008509)


![__results___29_3](https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/a71dc689-9d6e-4e68-b4a5-2b81420f2210)


![__results___29_4](https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/8b6ee2f4-5617-4ca7-a6c7-735bc40cb45d)



# The Metrics

![formula-MAE-MSE-RMSE-RSquared](https://github.com/UKVeteran/Seoul-Bike-Sharing-Demand-Prediction/assets/39216339/7da211ec-7ca8-4c91-9be8-4e36966a3324)


# Stack'em Up

Stacked Model - Train R-squared: 0.99<br>
Stacked Model - Test R-squared: 0.91<br>
Stacked Model - Test RMSE: 3.31



```python

from sklearn.ensemble import StackingRegressor
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import r2_score, mean_squared_error
from math import sqrt

# Split your data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Define the base models
base_models = [
    ('LinearRegression', LinearRegression()),
    ('Lasso', Lasso()),
    ('Ridge', Ridge()),
    ('KNeighborsRegressor', neighbors.KNeighborsRegressor()),
    ('SVR', SVR(kernel='rbf')),
    ('DecisionTree', DecisionTreeRegressor(random_state=42)),
    ('RandomForest', RandomForestRegressor(random_state=42)),
    ('ExtraTreeRegressor', ExtraTreesRegressor(random_state=42)),
    ('GradientBoostingRegressor', GradientBoostingRegressor(random_state=42)),
    ('XGBRegressor', xgb.XGBRegressor(random_state=42)),
    ('LightGBM', lightgbm.LGBMRegressor(num_leaves=41, n_estimators=200, random_state=42)),
    ('MLPRegressor', MLPRegressor(activation='logistic', solver='sgd', learning_rate='adaptive', max_iter=1000, learning_rate_init=0.01, alpha=0.01))
]

# Define the meta-model (stacking regressor)
meta_model = LinearRegression()

# Create the stacking regressor
stacked_model = StackingRegressor(estimators=base_models, final_estimator=meta_model)

# Train the stacking model on the training data
stacked_model.fit(X_train, y_train)

# Make predictions using the stacked model
stacked_train_predictions = stacked_model.predict(X_train)
stacked_test_predictions = stacked_model.predict(X_test)

# Calculate R-squared and RMSE for the stacked model
train_r2 = r2_score(y_train, stacked_train_predictions)
test_r2 = r2_score(y_test, stacked_test_predictions)
test_rmse = sqrt(mean_squared_error(y_test, stacked_test_predictions))

print(f"Stacked Model - Train R-squared: {train_r2:.2f}")
print(f"Stacked Model - Test R-squared: {test_r2:.2f}")
print(f"Stacked Model - Test RMSE: {test_rmse:.2f}")
```
