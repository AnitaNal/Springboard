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
+ 
