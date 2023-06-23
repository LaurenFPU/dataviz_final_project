---
title: "Mini-Project 02"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
---

# Data Visualization Project 02

Two data sets (and a county shapesfile) were used to make the following visualizations:

- [2015 Pesticide Use in U.S. Agriculture](https://www.kaggle.com/datasets/usgs/pesticide-use)
  - Visualization 1 - **an interactive plot** (*2015 Estimated Pesticide Use by Florida County*)
  - Visualization 2 - **a spatial visualization** (*2015 Estimated Pesticide Use by Florida County*)
- [Statlog heart disease dataset](https://www.kaggle.com/datasets/shubamsumbria/statlog-heart-data-set)
  - Visualization 3 - **coefficients of a linear model** (*Estimated impact of 13 factors on predicting heart disease*)
  
The Statlog dataset was originally accessed through the [UCI repository](https://archive.ics.uci.edu/ml/datasets/statlog+(heart)). It currently appears to be broken/missing. The first (Kaggle) link above for the *Statlog heart disease dataset* does currently work.

## **2015 Estimated Pesticide Use by Florida County**

### ***an interactive plot***

The below plot is static and has been reduced in size. For a larger and interactive version, click [here](FL_pests_interactive.html). 











![](FL_pest_interactive.png){width=675px, height=600px}

The above plot shows the estimated total pesticide use by Florida counties in 2015. The top 20 counties are in red and the bottom in blue. Unsurprisingly, highly agricultural counties use more pesticides than others. This was confirmed by referencing the 2017 agricultural market values from the [Florida Department of Agriculture and Consumer Services](https://www.fdacs.gov/Agriculture-Industry/Florida-Agriculture-Overview-and-Statistics).

### ***a spatial visualization***



<img src="laurenFPU_project_02_files/figure-html/map-FLpests2015-1.png" width="80%" />


The above map shows the same information as the first visualization, but we are now able to recognize areas of Florida that may be difficult to distinguish in a list of county names. 

There is little to no pesticide use in the 

- Keys
- Everglades

There is also very little use in the pan handle near Apalachicola National Forest. 

Most of the pesticide use is in central to south-central Florida. Quite likely this map indicates the density of farmland in Florida counties. An interesting extension to would be to normalize the results by farmland acreage. 


## **Estimated impact of 13 factors on predicting heart disease**

### ***Modeling Heart Disease*** - predictor significance




<img src="laurenFPU_project_02_files/figure-html/heart-coefs-plot-1.png" width="60%" />


For the third visualization, the Statlog Heart Disease data set was used (the same data set used in mini-project 1). A linear model was made to predict heart disease (positive versus negative) using all thirteen available predictors. In the above coefficients plot, we see that 

- sex,
- number of vessels colored by fluoroscopy,
- exercise induced angina,
- chest pain type,
- and fasting blood sugar

are the most significant predictors. 

Interestingly, 

- resting blood pressure,
- serum cholesterol,
- age,
- and maximum heart rate

appear to have very little predictive power. 

The linear model created was very simple and is not recommended for prediction purposes. However, the coefficients plot can be used in additional analysis and feature selection. 

---
