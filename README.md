# Regression Analysis of CDC Guidelines on Muscle-Strengthening Activity
Authors: Kevin He, Ash Dubal, Jason Gill
## Table of Contents
 - [Abstract](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#abstract)
 - [Introduction](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#introduction)
 - [Data Cleaning and Description](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#data-cleaning--description)
     - [Data Cleaning](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#data-cleaning)
     - [Data Distribution](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#data-distribution)
     - [Exploratory Analysis](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#exploratory-analysis)
 - [Methods](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#methods)
     - [Hypothesis Testing and Model Comparision](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#hypothesis-testing-and-model-comparison)
     - [Model Building](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#model-building)
 - [Results](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#results)
     - [Cross Validation and Test Data](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#cross-validation-and-test-data)
     - [Residual Analysis](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#residual-analysis)
 - [Conclusion](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#conclusion)
 - [References](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#references)
 - [Appendix](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis#appendix)



## Abstract
Adult obesity has been a long and growing issue in the United States (US) with some calling it an "obesity epidemic". The US Centers for Disease Control and Prevention (CDC) publishes guidelines on both the amount of moderate intensive activity and muscle-strengthening activity required for adults to mitigate the risks associated with obesity. In this analysis, we investigate the effects of year, state/district of residence, and age on the proportion of adults that satisfy CDC guidelines on muscle-strengthening activity. We performed an exploratory analysis to visualize the distribution of the data, conducted modelling using ordinary least squares and stepwise regression with 10-fold cross-validation for model selection, and carried out residual analysis. An ordinary least squares model was developed that explained 88% of the variability, and all three factors of interest were shown to be statistically significant.


## Introduction

The CDC in the US recommends that adults need at minimum 150 minutes of aerobic activity and at least two days of muscle-strengthening activity per week, but only 1 in 4 adults in the US and 1 in 5 high school students satisfy this recommendation. With obesity being a growing public health issue in the US, it is more than critical that Americans meet these recommendations by the CDC, as there are harmful effects associated with not getting enough physical activity. These effects include being at a higher likelihood of developing high blood pressure, high blood cholesterol, type 2 diabetes, heart disease, and cancer; while the benefits vastly improve quality of life with improved sleep, cognitive ability, musculoskeletal health, and a reduced risk of dementia.

Using the nutrition data provided by the CDC, we investigate the proportion of adults that satisfy CDC guidelines on muscle-strengthening activity, and see if the year, age, and state/district of residence linearly affect the topic at hand. Since the data collected for this study only include the years 2011, 2013, and 2015, we are also interested to see if there has been a change in proportions of adults engaging in muscle-strengthening activity over the years. In this study, we will investigate and determine what factors contribute to satisfying the muscle-strengthening guidelines by the CDC, and see how the different variables are related.

## Data Cleaning & Description

#### Data Cleaning
The nutrition data consists of multiple different survey results investigating the different socio-economic factors and their effects on a multitude of survey questions. The survey is set up so that only one measure of socio-economic status is recorded for each question.

We begin cleaning the data set by removing all redundant variables, footnotes, confidence intervals, and sample sizes for each survey. Furthermore, we see that there is no difference between the start and the end year of the surveys, so we remove the end year. Finally, we filter the data set to surveys that include our question of interest: the proportion of adults who engage in muscle-strengthening activity on two or more days a week. As well, we narrow our focus to only US states and districts since not enough data was available for US territories.

After we cleaned and filtered our data, we are left with four variables.

|Variable Name| Description|
|-------------|------------|
|Data_Value| The proportion of adults that satisfy CDC guidelines on muscle-strengthening activity|
|YearStart| The year the survey was conducted |
| LocationDesc | Names of the 50 US states and the District of Columbia |
|Age.years. | Age grouped into 6 factors: (18-24), (25-34), (35-44), (45-54), (55-64), (65+) |

Where we are trying to predict Data_Value using year, state/district, and age.

Before we begin our exploratory analysis, we introduce the data point (YearStart = 2011, LocationDesc = Alabama, Data_Value = 70, Age.years. = 18 - 24) as an outlier point to see how it would impact our regression models.

#### Data Distribution
![](https://res.cloudinary.com/kevinhe/image/upload/v1607295476/STAT%20350%20Project/distribution_aoi6bn.png)

We start by analyzing the distribution of each variable left after cleaning the data. Due to the long name of `proportion of adults that satisfies CDC guidelines on muscle-strengthening activity`, in our plots, we refer to this study question simply as `proportion`. From the above bar charts, it is clear that the surveys are evenly distributed throughout the years, state/district, and age, except for the year `2011`, the state of `Alabama`, and the age group `18 - 24`, as they all have one additional count due to the newly introduced data point. As well, when looking at the density of the proportion of adults that satisfy CDC recommendations for muscle-strengthening activity, it is generally right-skewed and has a large peak that is slightly above 25 and a smaller spike around 43.

#### Exploratory Analysis
We now investigate the relationship between each explanatory variable with the response variable.

![](https://res.cloudinary.com/kevinhe/image/upload/v1607295753/STAT%20350%20Project/exploratory_analysis_b8tov0.png)


We start by exploring the change in the proportion of adults that satisfy CDC guidelines for muscle-strengthening activity throughout the years of the study. Each year has a similar IQR with the distribution slightly increasing as the years advance, which implies a linear relationship. The new point is an extreme outlier for the 2011 boxplot.

When we compare the age groups to the proportion of adults that satisfy CDC guidelines for muscle-strengthening activity, the relationship becomes clear. We see a negative linear relationship showing that younger adults are more likely to engage in muscle-strengthening activity in comparison to older adults/seniors. An interesting observation with this boxplot is that the majority of outlier points have a very low proportion value, suggesting that there are states/districts where very few adults are getting the muscle-strengthening activity that they need.

We then compare to see if there is a difference in proportion by state/district. This boxplot is arranged by increasing medians and there is clear evidence that the proportions are different by state, as the IQR for West Virginia and Colorado has no overlapping values. West Virginia has the lowest median proportion being just under 20, while Colorado has the highest median value being over 30. To visualize this clearer, we have also produced a heat map of the proportions by state/district below.

![](https://res.cloudinary.com/kevinhe/image/upload/v1607311266/STAT%20350%20Project/heatmap_ty7bkv.png)

![](https://res.cloudinary.com/kevinhe/image/upload/v1607488135/STAT%20350%20Project/pairs_ejnjul.png)

Lastly, we look at the pairs plot to check for relationships between each variable. When looking at plots between explanatory variables, there is not a linear relationship as the data appears to be uniform. When comparing each explanatory variable to Data_Value, we see a clear linear relationship with Age.years. and a slight linear relationship with YearStart. The plot for LocationDesc and Data_Value is harder to interpret, but it is evident that Data_Value changes by state/district as the range in Data_Value shifts for each level in LocationDesc.

## Methods

Before we build our regression models, we first split the data into a 80/20 train/test split for model training and testing.

#### Hypothesis Testing and Model Comparison

We want to know if the slopes of the regression line depend on the different factor levels of age group and state/district, given the intercept remains constant. We train a linear regression model with no interaction terms on the training data, as well as create models with interaction terms on age group, state/district, and both age group and state/district also using the training data. The result of our hypothesis test suggests that the slopes are dependent on both the different levels of age group and state/district at alpha = 0.05. The models used for these tests and the p-value for each test is reported below.

|Model| Description|
|-----|------------|
|Linear Regression | Data_Value ~ YearStart + Age.years + Location Desc |
|Linear Regression with Interaction Term `YearStart:Age.years` | Data_Value ~ YearStart + Age.years + Location Desc + YearStart:Age.years |
|Linear Regression with Interaction Term `YearStart:LocationDesc` | Data_Value ~ YearStart + Age.years + Location Desc + YearStart:LocationDesc |
|Linear Regression with Both Interaction Terms  `YearStart:LocationDesc` and `YearStart:Age.years` | Data_Value ~ YearStart + Age.years + Location Desc + YearStart:Age.years + YearStart:LocationDesc|

| Anova Test | P-Value|
|------------|--------|
| anova(Linear Regression, Linear Regression with Interaction Term `YearStart:Age.years` ) | 0.02277 |
| anova(Linear Regression, Linear Regression with Interaction Term `YearStart:LocationDesc`) | 0.04196 |
|anova(Linear Regression with Interaction Term `YearStart:Age.years`,  Linear Regression with Both Interaction Terms on `YearStart:LocationDesc` and `YearStart:Age.years`) | 0.03635 |



Due to the fact that we are primarily concerned with the predictive power of each model and since adding interaction terms affects the bias-variance tradeoff, we compare these models using AIC values.

|Model| AIC |
|-----|-----|
|Linear Regression | 3506.098	|
|Linear Regression with Interaction Term `YearStart:Age.years` | 3501.860		|
|Linear Regression with Interaction Term `YearStart:LocationDesc` | 3528.803|
|Linear Regression with Both Interaction Terms  `YearStart:LocationDesc` and `YearStart:Age.years` | 3523.032|

Here we see that despite the slopes being different for each level of age group and state/district, the model with only the interaction term on age performs the best according to the AIC value with the model with no interaction terms following closely behind.

#### Model Building

We now start to build our regression models. Using the Caret package, for each model we perform 10-fold cross-validation using our training data. We then save the computed R^2 value and the root-MSPE for each fold in each model.

Although we have completed hypothesis testing and model selection on the training data to test if interaction terms are necessary, we still use the models that we have rejected. Specifically, we fit the following models using all explanatory variables:

- linear regression
  - Data_Value ~ YearStart + Age.years + Location Desc
- linear regression with interaction term `YearStart:Age.years`
  - Data_Value ~ YearStart + Age.years + Location Desc + YearStart:Age.years
- linear regression with interaction term `YearStart:LocationDesc`
  - Data_Value ~ YearStart + Age.years + Location Desc + YearStart:LocationDesc
- linear regression with both interaction terms  `YearStart:LocationDesc` and `YearStart:Age.years`
  - Data_Value ~ YearStart + Age.years + Location Desc + YearStart:Age.years + YearStart:LocationDesc

For each one of the models listed above, we also perform stepwise variable selection for a total of 8 models.

## Results

#### Cross-Validation and Test Data

With the R^2 and the root-MSPE saved from the 10-fold cross-validation results, we create boxplots to compare the models.

![](https://res.cloudinary.com/kevinhe/image/upload/v1607488135/STAT%20350%20Project/MSPE_wsvya3.png)

From the left boxplot containing the root-MSPE values, it is evident that none of the stepwise regression models performed as well as the linear regression models as they have the largest root-MSPE values. When comparing the linear regression models, the model with no interaction terms performs the best as it has one of the lowest median root-MSPE values and a low boxplot range. The model with the interaction on age group follows closely behind, but it has the highest median root-MSPE value out of the linear regression models.

We also produced boxplots for the R^2 value from the cross-validation results. The two best models are the linear regression model with no interaction terms and the linear regression model with interaction on age group. They both have around the same median R^2 value and IQR, but we select the model without interaction terms as it does not have long boxplot whiskers and it is a simpler model.


Finally, we see how each model performs on new data, specifically our test data set. We get the following results:

|Model| MSPE |
|-----|-----|
|Linear Regression - No Interaction |  10.30324 |
|Linear Regression - Age Group Interaction | 10.36737	|
|Linear Regression - LocationDesc Interaction | 10.48252 |
|Linear Regression - Age Group and LocationDesc Interaction | 10.5332 |
|Stepwise Regression - No Interaction |  27.18744	|
|Stepwise Regression - Age Group Interaction |  27.18744	|
|Stepwise Regression - LocationDesc Interaction |  27.18744  |
|Stepwise Regression - Age Group and LocationDesc Interaction |27.18744 |

As expected from the cross-validation results, the linear regression model with no interaction terms performs the best with the linear regression model with a interaction on age group being close behind. This result contradicts the conclusion of our hypothesis tests to see if the slopes depended on the different levels of LocationDesc and Age.years., but is backed up by our original model comparison using AIC, as it has one of the best AIC values. This discrepancy could be explained by the bias-variance tradeoff that we briefly touched on before, as although we have 735 observations in our training data, we also have 51 levels in the LocationDesc variable and 6 levels in the Age.years. variable; so in actuality, when we create interaction terms, we have to estimate the coefficients for 306 additional regressors, which decreases the bias of the model as we have a more flexible model, but in turn, our variance increases drastically, making our models more variable.

From both the cross-validation and the test data results, we select the linear regression model with no interaction terms to be our best model. We also include a plot of the predicted vs observed values from our final model.

![](https://res.cloudinary.com/kevinhe/image/upload/v1607372154/STAT%20350%20Project/predicted_zpiswp.png)

From this we're able to see that the predicted results are all scattered evenly around the y = x line with a few potential outlier points.


#### Residual Analysis
Our final task in this analysis is to ensure that our final model satisfies the assumptions for linear regression.

We first see if there is a linear relationship between at least one of the explanatory variables to the response by running a summary of the model. The resulting p-value from the significance of linear regression test is < 2.2e-16. Using alpha = 0.05, we conclude that the response is linearly related to at least one of the explanatory variables.

We now perform analysis on the residuals.
![](https://res.cloudinary.com/kevinhe/image/upload/v1607216516/STAT%20350%20Project/residuals_aiqgwj.png)

 By looking at the Residual vs Fitted plot, all the points are randomly scattered with a mean of 0 with no clear pattern, which suggests that the constant variance assumption is satisfied.

The Normal Q-Q plot, for the majority, follows a straight line with very few points around the edges of both extremes deviating from the line. From this observation, the normality of the residuals is satisfied.

For the standardized residuals vs leverage plot, the points for leverage are scattered evenly around the same area with no points having a large leverage. As well, there exists a couple of points with standardized residuals beyond +/- 3, but since they do not have a high leverage point, they are not considered to be influential.

To double check that we do not have any influential points we use both Cook's Distance and diag(H) with standardized residuals, of which no points were identified.

Lastly, we use the variance inflation factor to see if there exists an issue with multicollinearity in the model, and since all the values are around 1, we conclude that there are no issues.

## Conclusion
  Overall, our final model, the linear regression model with no interaction terms, has an R^2 value of 0.8891376 which performs great given the number of variables that were filtered from the data set. Running one last summary on our chosen model we are able to answer the questions posed in the beginning of the analysis. We first see that as the year increases, there has been an improvement in the proportion of adults that satisfies CDC guideline on muscle-strengthening activity, although this change is quite marginal with a 0.38% increase per year from 2013 to 2015; this variable also poses as a limitation in our model which will be discussed later on. As well, we can see from the model that younger age groups are more likely to satisfy CDC guidelines on muscle-strengthening activity, with about a 3% decrease in proportion when moving up an age group. Finally, we are able to conclude that the likelihood of satisfying CDC guidelines on muscle-strengthening activity depends on the state/district of residence with states such as Hawaii, Alaska, Virginia, and Colorado having some of the largest increases for the intercept of the model, which is consistent with our exploratory analysis.

  Some of the limitations of this study include having YearStart as one of the variables in our model as we do not know if the upwards trend will continuously be linear or if the increase will slow down as time goes on, forcing us to extrapolate for predictions outside of the year range 2013 - 2015. Moreover, including 51 levels of state/district and 6 levels of age group impacts our model as we have to fit a coefficient for each level which increases model complexity. In future studies, state/district could be grouped into the 9 census regions or into Democratic and Republican controlled state/district to reduce their model complexity and to investigate further questions related to the data.

## References
- [Physical Activit Recommendations for Different Age Groups - CDC](https://www.cdc.gov/physicalactivity/basics/age-chart.html)
- [Lack of Physical Activity - CDC](https://www.cdc.gov/chronicdisease/resources/publications/factsheets/physical-activity.htm)
- [Physical Activity - CDC](https://www.cdc.gov/physicalactivity/basics/adults/index.htm#:~:text=Physical%20activities%20to%20strengthen%20your,addition%20to%20your%20aerobic%20activity.)

## Appendix
For the full code and analysis on this project [click here](https://github.com/kaishuun/Work-Out-Analysis/blob/main/Project.Rmd). For reproducibility purposes the seed `2928820` is set before any stochastic processes began.

For the data used in the analysis [click here](https://github.com/kaishuun/CDC-Physical-Activity-Regression-Analysis/blob/main/nutrition_data.csv).
