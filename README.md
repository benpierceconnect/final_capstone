# Predicting Hourly Bike-Sharing Demand
**Author:** Ben Pierce

## Research Question

How accurately can hourly bike rental demand be predicted using time, calendar, seasonal, and weather-related factors, and which of these factors are most strongly associated with demand?

## Project Overview

I applied data cleaning and preparation by checking for missing values, duplicates, and formatting issues. I explored patterns related to weather, time of day, holidays, weekends, and working hours. For modeling, I used Linear Regression, Ridge, and LASSO with chronological cross-validation and GridSearchCV, and measured performance with MAE, RMSE, and R-squared.

## Data

The two files are `hour.csv` and `day.csv`.

- `hour.csv` shape: 17,379 rows and 17 columns
- `day.csv` shape: 731 rows and 16 columns
- Date range: January 1, 2011 through December 31, 2012

## Data Quality and Cleaning

Fortunately, the actual files don't contain any missing values and no duplicates. This means we are keeping all 17,379 hourly records and all 731 daily records.

The hourly and daily totals were merged to make sure they checked out. A zero difference confirms that the two files agree.

## Feature Engineering

To begin we want to have a readable context of the values. We are mapping the fields to readable labels.

The engineered `hour_workingday` field is utilized for the model to represent different patterns by hours for working and non-working days.

## Exploratory Data Analysis

This stage is an opportunity to become more familiar with the datasets, and to put them into perspective and perhaps find patterns or avenues that will help us in meeting our objective.

The analysis includes:

- Distribution of hourly demand
- Daily rental trends
- Average demand by hour
- Working days compared with weekends and holidays
- Day-of-week and hour heatmap
- Season and weather
- Year and month
- Temperature, humidity, and wind speed
- Correlation matrix
- Outlier analysis

## Main EDA Findings

- Demand peaks at 17:00 with 461.5 rentals per hour.
- Working days show commute peaks with 525.3 rentals at 17:00.
- Weekends and holidays have a peak around 13:00.
- Rentals had an increase of 64.9% from 2011 to 2012.
- Fall has the highest demand in the four seasons. Spring has the lowest.
- Clear or partly cloudy weather has a higher demand than light rain or snow.
- Temperature has a positive relationship to demand, and humidity has a negative relationship.

## Outlier Analysis

I decided to utilize the IQR method on the hourly target. The method found 505 outliers, representing 2.91% of the hourly file. About 81% occurred at 8:00, 17:00, or 18:00, and about 82% occurred on working days.

I'm going to keep them because I think they help us identify patterns in other areas.

## Linear Regression Baseline

Because the `cnt` field is continuous, Linear Regression is used as the initial baseline.

The split is 80/20. The first 80% of unique dates is for training, and the remaining is for testing.

A pipeline is applied to process the model using scikit-learn.

I am going to use MAE as the primary metric because it reports prediction error in rentals per hour.

## Results

| Metric | Training | Testing |
|---|---:|---:|
| MAE | 51.303 | 78.225 |
| MSE | 4,992.989 | 11,026.488 |
| RMSE | 70.661 | 105.007 |
| R-squared | 0.821 | 0.773 |

The chronological test results are as follows:

- MAE: 78.23 rentals per hour
- MSE: 11,026.49
- RMSE: 105.01 rentals per hour
- R-squared: 0.773
- Linear Regression, Ridge, and LASSO produced nearly identical test results, with an MAE of about 78 rentals per hour and an R-squared of 0.773.
- Linear Regression remained the preferred model because it had the lowest cross-validation RMSE and regularization did not meaningfully improve performance.

Residuals are determined by taking the actual and subtracting the predicted. Positive residuals show under-prediction, while negative residuals show over-prediction.

## Final Conclusions

The three models predicted similar hourly demands, with an MAE of about 78 rentals/hr and an R-squared of 0.773. Linear Regression remained the best model because it had the lowest cross-validation RMSE and was the simplest.

Time of day and working-day status were important. Season, weather, temperature, and humidity also helped explain the demand. However, the model created 28 negative predictions and underpredicted demand above 600 rentals by about 227 rentals on average.

## Next Steps

Future work should focus on improving predictions during very high-demand periods, especially the working-day peaks around 8:00, 17:00, and 18:00.

## Project Notebook

- [Capstone.ipynb](Capstone.ipynb)
