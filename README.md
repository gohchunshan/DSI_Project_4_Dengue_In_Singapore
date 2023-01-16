# DSI_Project_4_Dengue_In_Singapore
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
1. 

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


## Modelling

#### Metrics for model evaluation
  
|---|Parameters|Train score|Test score|
|---|---|---|---|
|Linear Regression (no regularization)|lr__n_jobs: 1|0.107|0.222|
|Linear Regression (Ridge)|rd__alpha: 65.0|0.217|0.237|
|Linear Regression (Lasso)|la__alpha: 0.005</n>la__max_iter: 100000|0.246|0.242|
|Linear Regression (Elastic Net)|en__alpha: 0.01</n>en__l1_ratio: 0.8|0.237|0.228|
|Random Forest Regressor|rrf__n_estimators: 8|0.223|0.226|
|Gradient Boosting Regressor|gb__n_estimators: 330|0.324|0.312|
|KNeighbours Regressor|knn__n_neighbors: 6|0.112|0.114|

#### Final model

## Conclusion and Recommendations
