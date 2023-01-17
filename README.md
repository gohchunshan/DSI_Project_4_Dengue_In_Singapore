<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 20px; height: 55px">
# DSI Project 4 Dengue In Singapore
For the requirements of General Assembly Data Science Immersive Flex-7


## Problem Statement

You just graduated from General Assembly Data Science Immersive program and managed to secure an interview with the National environmental Agency (NEA). You have been assigned into a group with other interviewees and was given a take home BI test. You will be required to work together and present as a group during the interview. Time to look at the test!
Dengue fever [https://www.healthhub.sg/a-z/diseases-and-conditions/192/topic_dengue_fever_MOH] is spread though the bite of the Aedes mosquito. To prevent dengue fever, one must prevent the breeding of the Aedes mosquitoes. Aedes mosquitoes are identified by the black and white stripes on their bodies. It prefers to breed in clean, stagnant water easily found in our homes. You can get rid of the Aedes mosquito by frequently checking and removing stagnant water in your premises. The NEA has a stringent B-L-O-C-K STOP Dengue Now campaign.
In this exercise, you are required to predict Dengue Cases in Singapore. You will be using the following potential predictors for dengue:
Weather information: Rainfall and temperature patterns are one of the main drivers of dengue transmission as mosquitoes require standing water to reproduce
Google trends: Search terms containing the term ‘dengue’ have been successfully used to predict dengue cases in a number of countries. This is probably because those who are more susceptible to the virus, are also more likely to be googling about it.

#### Detailed problem statement

## Project Planning
A Gantt chart was used to track the progress of each task, which were split between team members

## Data Sources
1. Dengue clusters
2. Weather
3. Google Trends

## EDA

Describe the data. What does it represent? What types are present? What does each data points' distribution look like? Discuss these questions, and your own, with your partners. Document your conclusions.

What kind of cleaning is needed? Document any potential issues that will need to be resolved.
Note: As you know, EDA is the single most important part of data science. This is where you should be spending most of your time. Knowing your data, and understanding the status of its integrity, is what makes or breaks a project.

## Data Cleaning

#### Steps taken for data cleaning
###### Dealing with missing values in Weather dataset
The simplest way would be to drop all the rows containing NA values, but that is not feasible as there are quite significant proportion of NA values in our data.
We observe there is approximately 30% missing data for columns related to Rainfall (Highest 30 Min, 60 Min and 120 Min Rainfall), and 60% missing data for columns related to Temperature (Mean, Max and Min Temperatures), and also 60% missing data for Wind speed columns (Mean Wind Speed (km/h), Max Wind Speed (km/h)).

###### Method for imputing NA values
- Rainfall: Take the mean of all the other values within the same year, month and Station. However, we note that there is no Rainfall data at all for 2012 - 2013, hence this is only possible for the year 2014 onwards.
- Temperature/ Wind Speed: Take the mean value of all other values within the same year, month and Station as well, so that the monthly averages are not affected. There may be higher number of NA's left after imputing as there are < 50% non-null values in these columns. As there are many other null values to be imputed, we will do a second round of imputation by using the mean of the similar months from other years. For the rest that are not yet imputed, we will fill with the global mean.

#### Checkpoint
So far, we have 2 main dataframes, the `Weather` dataframe and `Total Dengue cases` dataframe. Since the `Station` column in the `Weather` dataframe and the `town` in the `Total Dengue cases` dataframe contain different list of values, we need to match the geographic regions in order to merge into one dataframe.

We looked through the list of Weather Stations and Towns, and created a lookup list to be used for merging the dataframes. This allows us to keep more granular data to have more rows of data for the eventual model. 
- As there may be a case where weather stations are in regions where no dengue cluster was reported, or vice versa (dengue cases are reported in places where there was no weather station), we will take only the intersection of the 2 lists of locations. 
- Also, there could be cases of 'many-to-one' matching between the 2 lists of locations, for e.g. Tuas, Tuas South and Tuas West stations are all part of 'Tuas' region in the list of towns for the Total Dengue Cases dataframe.  For these 'many-to-one' weather station to town matches, we do some work to take the aggregate mean of the multiple weather stations mapped to the 1 town, before merging both dataframes on the town and the epiweek. This way, there is greater data retention.

## Data Dictionary
|Column name|Type|Numerical/ Categorical|Description|
|---|---|---|---|
|total_rainfall|float|Numerical|Weekly mean of average rainfall (mm) for the given Town|
|rainfall_30min|float|Numerical|Weekly Highest 30 Min Rainfall (mm) for the Town|
|rainfall_60min|float|Numerical|Weekly Highest 60 Min Rainfall (mm) for the Town|
|rainfall_120min|float|Numerical|Weekly Highest 120 Min Rainfall (mm) for the Town|
|temperature_mean|float|Numerical|Weekly Mean Temperature (deg C)|
|temperature_max|float|Numerical|Weekly Max Temperature (deg C)|
|temperature_min|float|Numerical|Weekly Min Temperature (deg C)|
|windspeed_mean|float|Numerical||Weekly Mean Wind Speed (km/h)|
|windspeed_max|float|Numerical||Weekly Max Wind Speed (km/h)
|Town|str|Categorical|Geographical Town in Singapore (e.g. ang mo kio, yishun)|
|Region|str|Categorical|Geographical Region of Singapore. Options include: North, South, East, West, Central|
|epi_week_year|str|Numerical|Epi week (e.g. 2020-W45)|
|start|datetime|Numerical|First date of the epiweek|
|dengue_cases|int|Numerical||Number of dengue cases in NEA-reported clusters|
|Year|int|Numerical|Year|
|Month|str|Categorical|Month of the year|
|Day|int|Numerical|Day of the month|
|freq_dengue_gtrends|float|Numerical|Google search trends for keyword “Dengue Fever” (Rescaled)|
|freq_wolbachia_gtrends|float|Numerical|Google search trends for “Wolbachia” (Rescaled)|


## Grid Search Modelling

#### Metrics for model evaluation
Using Robust Scaler

|---|Parameters|Train score|Test score|RMSE|
|---|---|---|---|---|
|Linear Regression (no regularization)|lr__n_jobs: 1|0.136|0.133|1.394|
|Linear Regression (Ridge)|rd__alpha: 65.0|0.137|0.134|1.394|
|Linear Regression (Lasso)|la__alpha: 0.005<, la__max_iter: 100000|0.137|0.133|1.395|
|Linear Regression (Elastic Net)|en__alpha: 0.01, en__l1_ratio: 0.8|0.134|0.131|1.396|
|Random Forest Regressor|rrf__n_estimators: 8|0.253|0.280|1.271|
|Gradient Boosting Regressor|gb__learning_rate: 0.12, gb__max_depth: 5, gb__n_estimators: 335|0.371|0.369|1.190|

#### Final model
|---|Parameters|Train score|Test score|RMSE|
|---|---|---|---|---|
|Gradient Boosting Regressor|gb__learning_rate: 0.08, gb__max_depth: 6, gb__n_estimators: 340|0.795|0.365|1.193|


## Conclusion and Recommendations
