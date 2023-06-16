# Bike Sharing Company BoomBikes
> The given dataset contains the bike sharing deatils of the BoomBikes company from 2018 and 2019. A US bike-sharing provider BoomBikes has recently suffered considerable dips in their revenues due to the ongoing Corona pandemic. The company is finding it very difficult to sustain in the current market scenario.

So company to understand the factors on which the demand for these shared bikes depends. Specifically, they want to understand the factors affecting the demand for these shared bikes in the American market.

Essentially, the company wants —

Which variables are significant in predicting the demand for shared bikes.
How well those variables describe the bike demands
To know the accuracy of the model, i.e. how well these variables can predict demand for shared bikes.


## Table of Contents
* [Python Libraries](#python-libraries)
* [Data Understanding](#data-Understanding)
* [Data Cleaning](#Data-Cleaning)
* [Dummy Variables](#Dummy-Variables)
* [Data Preparation](#data-Preparation)
* [Model Building](#Data-Building)
* [Residual Analysis](#Residual-Analysis)
* [Predictions](#Predictions)
* [Model Evaluation](#Model-Evaluation)
<!-- You can include any other section that is pertinent to your problem -->

## python-libraries
- To perform the EDA on the given dataset we are using python libraries like
  numpy, pandas
- For plotting the extracted data on different graphs we will using below libraries
  seaborn, matplotlib
- For builting a linear regression model we will using below libraries
  sklearn, statsmodel

<!-- You don't have to answer all the questions - just the ones relevant to your project. -->

## Data Understanding
- Shape
  Data set has 730 rows and 16 features.
- Details
  Dataset has important features which will be considered in this analysis.
  Continuous features: 'temp','atemp','hum','windspeed', 'cnt',
  Categorical features: 'season', 'yr', 'mnth', 'holiday', 'weekday', 'workingday', 'weathersit'.

<!-- You don't have to answer all the questions - just the ones relevant to your project. -->

## Data Cleaning
- Missing Values
  After checking the missing value count, it is found that there are no missing values in the dataset.
<!-- As the libraries versions keep on changing, it is recommended to mention the version of library used in this project -->

## Dummy Variables
- Dummy variables creation for categorical variables
  N-1 dummy variable need to be prepared to avoid multicollinearity.
  Code :
  season_dumm = pd.get_dummies(bikeshare['season'], drop_first = True)
  weathersit_dumm = pd.get_dummies(bikeshare['weathersit'], drop_first = True)
  month_dumm = pd.get_dummies(bikeshare['mnth'], drop_first = True)
  weekday_dumm = pd.get_dummies(bikeshare['weekday'], drop_first = True)
- Concating
  Concat the newly created dummy variables with the original dataframe.
  Code:
  bikeshare = pd.concat([bikeshare, season_dumm, weathersit_dumm, month_dumm, weekday_dumm], axis = 1)
- Drop original variable for which we have created dummy variables
  bikeshare.drop(['season','weathersit','mnth','weekday'], axis = 1, inplace = True)
  
## Data Preparation
- Feature `instant` need to be removed as it has unique values and has no significance in analysis.
- Feature `dteday` need to be removed as we already have yr and month column present in dataset.
- Features `casual` and `registered` as target variable cnt already has the values of both the variables
- The numeric values of features `season` and `weathersit` has no specific order significance, so we need to convert it back to its original categorical values.
- Also feature `weekday` and `mnth` need to be converted to its original categorical values.
- Using sklearn library split dataset data into training (70% data) and testing (30% data) sets
- Rescale the continuous features using min-max scaling for better interpretation.
<!-- You don't have to answer all the questions - just the ones relevant to your project. -->

## Model Building
- Divide training dataset into X and y datasets.
  X dataset will have all feature except output feature `cnt`.
  Y dataset will have only output feature `cnt`.
- Using RFE find first 20 significantly correlated variables.
- We will use only 20 features which were selected by RFE for model building.
- With 20 fetaures: R-squared = 0.852, Adj. R-squared = 0.845, F-statistic = 140.3. 
    Also there are few features with high p-value like `atemp` = 0.861, `feb` = 0.352, `aug` = 0.239, `May` = 0.189 need to be dropped one by one to observe the model performance.
- With 16 features: R-squared = 0.850, Adj. R-squared = 0.845, F-statistic = 175.1.
    We have all p-values less than 0.05.
    We need to check the VIF values for multicollinearity.
    The high VIF values for feature are `hum` = 29.59, `temp` = 18.31. These need to dropped one by one.
- After dropping `hum`, with 15 features: R-squared = 0.845, Adj. R-squared = 0.840, F-statistic = 179.5. 
    Still we have all p-values less than 0.05.
    After dropping `hum`, the VIF value for `temp` has reduced to 7.29 from 18.31. But it is still > 5.
    Since feature `temp` is more correlated with output variable (from graph) and has high importance in business, we need to keep it.
- We have finalized our predictors. There are total 15 final predictors. 

## Residual Analysis
- Predict the builded model and find the error terms.
- After plotting the histogram of the error terms we found that it respects the assumption of linear regression as error terms are normally distributed with mean 0. 
- After plotting scatter plot of error terms we found that error terms are independant of each other i.e., next error term is not dependant on previous error term.

## Predictions
- We nned to scale the test set using min-max scaling.
- Divide test data set into X dataset and Y dataset. 
- Predict the model for test X and Y datasets for final 15 selected features.

## Model Evaluation
- Plot y_test and above predicted y_pred to understand the spread. It shows linear relationship.
- If we check r2_score of both the y_test and y_pred dataset. It found as 0.8112961909300445.
- The r2_score of test dataset is 0.8112961909300445 and training dataset was 0.845. The diffrence is not more than 5%.


## Contact
Created by [@vaibhav-nk] - feel free to contact me!


<!-- Optional -->
<!-- ## License -->
<!-- This project is open source and available under the [... License](). -->

<!-- You don't have to include all sections - just the one's relevant to your project -->