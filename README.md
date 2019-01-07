# Springboard Capstone:  
A Spatial and Temporal Analysis of Crimes in Chicago
-----------
### Problem and Intent for Research
For the period of 2002-2016, Chicago has had, for the most part, crime rates above the U.S. average.  Not only does this make the city more unsafe, it also could endanger the lives of police officers.  In an effort to mitigate crime rates in Chicago and/or promoting officer safety, I tried to figure out any spatial/temporal factors that would be useful in predicting th etype of crime committed. 

### Procedure
1) Data Wrangling - Cleaned data and filled in missing values when possible.  Also added additional spatial and temporal features.
2) EDA - Performed a visual analysis of all features to see which ones had a relationship with the type of crime
3) Model Evaluation - Evaluated several models with the chosen features
4) Important Features - Examined the most important features based on the coefficients produced by the best model 

### Final Results
+ The SGD Classifier with class weight set to 'balanced' and loss set to 'log' performed the best when using data scaled with MinMaxScaler
+ Location description was the most important feature when looking at individual coefficients from the SGD Classifier
+ After averaging the coefficients for each type of crime and all dummy variables, the following features were the most important:  X/Y coordinates (longitude/latitude), day type (weekday/weekend and no holiday/federal holiday), distance from the city center of Chicago, season, time of day, day of the week, and the federal holiday. 

### Repository Structure
#### Scripts:
+ **ChicagoCrime_CleanData_Part1.ipynb**
  + Cleaned crime data and added columns such as day of the week, season, street....
+ **ChicagoCrime_CleanData_Part2.ipynb**
  + Filled in missing locations, wards, and communities
+ **CityCenter_Police.ipynb**
  + Added columns for distance from Chicago city center and closest police station
+ **Bus_Train_Stops.ipynb**
  + Added columns for distance from closest bus stop and train stop
+ **Liquor_Stores.ipynb**
  + Filtered liquor stores from Chicago business license data 
  + Added column for distance from closest 
+ **Holidays.ipynb**
  + Added columns for what federal holiday it was and if there was a holiday or not
+ **ChicagoCrime_EDA.ipynb**
  + Script for EDA
+ **Setup_Xy.ipynb**
  + Sampled 3 million reports and saved separte files for the features (X) and the target (y)
+ **Model_Evaluation.ipynb**
  + Evaluated several models and extracted important features
 
#### Documents
+ **Capstone_Proposal.pdf**
  + Initial capstone proposal 
+ **Capstone_DataWrangling.pdf**
  + Report documenting data wrangling procedures
+ **Capstone_MilestoneReport.pdf**
  + Report documenting data wrangling and EDA
+ **Capstone_FinalReport.pdf**
  + Final report documenting full procedure and final results
+ **Capstone_PowerPoint.pdf**
  + Final slides for capstone
