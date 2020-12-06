# Regression Analysis of CDC Guidelines on Muscle strengthening Activity

## Table of Contents
 - Add links later on github

## Abstract
Adult obesity has been a long and growing issue in the United States with some calling it an "obesity epidemic". The US Centers for Disease Control and Prevention publishes guidelines on both the amount of moderate intensive activities and muscle strengthening activities required for adults to mitigate the risks associated with obesity. In this analysis, we investigate the effects of year, State/District of residence, and age on the proportion of adults that satisfy CDC guidelines on muscle strengthening activity. We performed an exploratory analysis to visualize the distribution of the data, conducted modelling using ordinary least squares and stepwise regression with 10-fold cross validation for model selection, and carried out residual analysis. A model was developed that explained 88% of the variability, and all three factors of interest were shown to be statistically significant to the proportion of adults that satisfy CDC guidelines on muscle strengthening activity.


## Introduction

The Centers for Disease Control and Prevention (CDC) in the United States recommends that adults need at minimum 150 minutes of aerobic activity and at least two days of muscle-strengthening activities per week, but only 1 in 4 adults in the United States (US) and 1 in 5 high school students satisfy this recommendation **[CITE HERE]**. With obesity being a growing public health issue in the US, it is more than critical that Americans meet these recommendations by the CDC as there are harmful effects associated with not getting enough physical activity. These effects include being at a higher likelihood of developing high blood pressure, high blood cholesterol, type 2 diabetes, heart disease, and cancer, while the benefits vastly improve quality of life with improved sleep, cognitive ability, musculoskeletal health, and a reduced risk of dementia.

Using nutrition data provided by the CDC, our study population adults over the age of 18, and we are assuming independence for the collected data. We utilize regression approaches to investigate the relationship between the proportion of adults that satisfies CDC guidelines on muscle strengthening activity by the State/District that they reside and their age. Due to the long name of `proportion of adults that satisfies CDC guidelines on muscle strengthening activity`, in our plots we refer to this study question simply as `proportion`. Since the data is collected for this subject is only collected in 2011,2013, and 2015, we are also interested to see if there has been a change in proportions of adults engaging in muscle strengthening activities over the years. In this study we will investigate  and determine what factors contribute to satisfying the muscle-strengthening requirements by the CDC, and to see how the different variables are related.

## Data Cleaning & Description

#### Data Cleaning
The nutrition data consists of multiple different survey results investigating the different socio-economic factors and their effects on a multitude of survey questions. The survey is set up so that only one measure of socio-economic status is recorded for each survey question.

We begin cleaning the data set by removing all redundant variables that contain the same information as another variable, footnotes, confidence intervals, and sample sizes for each survey. Furthermore, we see that there is no difference between the start and the end year of the survey, so we remove the end year. Finally, we filter the data set to surveys that include our question of interest: proportion of adults who engage in muscle-strengthening activities on two or more days a week. As well, we narrow our focus to the only US States and Districts since not enough data was available from US territories.

After we cleaned and filtered our data we are left with four variables

|Variable Name| Description|
|-------------|------------|
|Data_Value| The proportion of adults that satisfy CDC guidelines on muscle strengthening exercise|
|YearStart| The year the survey was conducted |
| LocationDesc | Names of the 50 US States and the District of Columbia |
|Age.years. | Groups ages into 6 categories (18-24),(25-34),(35-44),(45-54),(55-64),(65+) |

Where we are trying to predict Data_Value using year, State/District, and age.

Before we begin our exploratory analysis, we introduce the data point (YearStart = 2011, LocationDesc = Alabama, Data_Value = 70, Age.years. = 18 - 24) as an outlier point to see how it would impact our regression models.


#### Data Distribution
![](https://res.cloudinary.com/kevinhe/image/upload/v1607203809/STAT%20350%20Project/distribution_t5jfwc.png)

We start with analyzing the distribution of each one of these variables. From the above bar charts, it is clear that the surveys are evenly distributed throughout the years, State/District, and age; with the extra data point that we have added, the year `2011`, the State of `Alabama`, and the age grouping `18 - 24` each having one additional point for each of the explanatory variable bar charts. As well, when looking at the density of the proportion of adults that satisfy CDC recommendations for muscle-strengthening activities, it has a large peak that is slightly above 25, and a smaller spike around 43, and is generally right-skewed, potentially suggesting a bimodal distribution.

#### Exploratory Analysis
We now investigate the relationship between each explanatory variable with the response variable, meaning we would able to explain the variability of the response variable using the explanatory variable.

![](https://res.cloudinary.com/kevinhe/image/upload/v1607204812/STAT%20350%20Project/exploratory_analysis_mzpemu.png)


We start by exploring the change in the proportion of adults that satisfy CDC guidelines for muscle strengthening activity. Each year has a similar IQR with the distribution slightly increasing as the years advance, which implies a linear relationship. In this boxplot, the new point is an extreme outlier for the 2011 boxplot, otherwise, there exist very few outliers in this boxplot.

When we compare the age groups to the proportion of adults that satisfy CDC guidelines for muscle strengthening activity the relationship becomes clear. As age increases, we see a  negative linear relationship showing that younger Americans are more likely to work out in comparison to older Americans. An interesting observation with this boxplot is that there exist more outlier points of Americans who engage in fewer muscle-strengthening activities corresponding to their age grouping than Americans who engage in more muscle-strengthening activities within their age group.

We compare to see if there is a difference in the proportion of Americans that engage in muscle-strengthening activities on two or more days by State. This boxplot is arranged by increasing medians and there is clear evidence that the proportions are different by State, as the IQR for West Virginia and Colorado has no overlapping values. West Virginia has the lowest median proportion being just under 20, while Colorado has the highest median value over 30. To visualize this clearer on a map, we have also produced a heat map of the proportion by State/District below.

![](https://res.cloudinary.com/kevinhe/image/upload/v1607205209/STAT%20350%20Project/heatmap_xgcqvw.png)

![](https://res.cloudinary.com/kevinhe/image/upload/v1607205508/STAT%20350%20Project/pairs_goykfv.png)

Lastly, we look at the pairs plot to check for relationships between each variable. When looking at the layout between explanatory variables there isn't a linear relationship as the data appears to be uniform. When comparing each explanatory variable to Data_Value, we see a clear linear relationship with Age.years. and a slight linear relationship with YearStart. The pairs plot of LocationDesc is harder to interpret, but the data primarily looks randomly distributed.

## Methods

Before we build the regression models, we first split the data into a 80/20 train/test split for model testing and training.

#### Hypothesis Testing and Model Comparison

We want to know if the slopes of the regression line depend on the different factor levels for the age groups and the State/District. We train a linear regression model with no interaction terms on the training data, as well as create models with interaction terms on age groups, State/District, and both age groups and State/District also using the training data. The result of our hypothesis test suggests that the slopes are only different with the factor on age groups at alpha = 0.05.

Since we are primarily concerned with the predictive power of each model and since adding interaction terms affects the bias-variance tradeoff of the model, we perform model selection using AIC values.

|Model| AIC |
|-----|-----|
|Linear Regression - no interaction | 3506.098	|
|Linear Regression - Age group interaction | 3501.860		|
|Linear Regression - LocationDesc interaction | 3528.803|
|Linear Regression - Age group and LocationDesc interaction | 3523.032|

Here we see that despite the slopes being different for each level of age group and State/District, the model with only the interaction term on age performs the best according to the AIC value with the model with no interaction terms following closely behind.

#### Model Building

We now start to build our regression models and start tuning parameters if necessary. Using the Caret package, for each model we make we perform 10-fold cross-validation using our training data. We then save the computed R^2 value and the root-MSPE for each fold in each model.

Although we have completed hypothesis testing and model selection on the training data to test if interaction terms are necessary, we still use the models that we have rejected. Specifically we fit the following models using all explanatory variables:

- linear regression
- linear regression with interaction term `YearStart:Age.years`
- linear regression with interaction term `YearStart:LocationDesc`
- linear regression with both interaction terms on `YearStart:LocationDesc` and `YearStart:Age.years`

For each one of the models listed above, we also perform stepwise variable selection with parameter tuning from using 10-Fold Cross Validation for a total of 8 models.

## Results

#### Cross-Validation and Test Data Results

With the R^2 and the root-MSPE saved from the 10-fold cross-validation results, we create boxplots to compare the models.

![](https://res.cloudinary.com/kevinhe/image/upload/v1607215843/STAT%20350%20Project/MSPE_fiwkkl.png)

From the left boxplot of root-MSPE values, it is evident that none of the stepwise regression models performed as well as the linear regression models as they have large root-MSPE values. When comparing the linear regression models, the linear regression model with no interaction terms performs the best as it has one of the lowest median root-MSPE values and a low boxplot range. The linear regression model with interaction on age follows closely behind as it has the highest median root-MSPE values out of the linear regression models.

We also produced boxplots for the R^2 value in each fold of all models. The two best models are the linear regression model with no interaction terms and the linear regression model with interaction on age groups, they both have around the same median R^2 value and IQR, but we select the model without interaction terms as it does not have long boxplot whiskers.


Finally, we see how each model performs on new data, specifically our test data set. We get the following results:

|Model| MSPE |
|-----|-----|
|Linear Regression - no interaction |  10.30324 |
|Linear Regression - Age group interaction | 10.36737	|
|Linear Regression - LocationDesc interaction | 10.48252 |
|Linear Regression - Age group and LocationDesc interaction | 10.5332 |
|Stepwise Regression - no interaction |  27.18744	|
|Stepwise Regression - Age group interaction |  27.18744	|
|Stepwise Regression - LocationDesc interaction |  27.18744  |
|Stepwise Regression - Age group and LocationDesc interaction |27.18744 |

As expected from the cross validation results, the linear regression model with no interaction terms performs the best with the linear regression model with no interactions being close behind. This result contracts the results of our hypothesis tests to see if the slopes depended on the different levels of LocationDesc and Age.years. , but is backed up by our original model comparison using AIC as it has one of the top AIC values in comparison to the other models. This discrepancy could be explained by the bias-variance trade off as although we have 735 data points, we also have 51 levels in the LocationDesc variable and 6 levels in the Age.years. variable so in actuality, when we create interaction terms we have to estimate the coefficients for 306 additional variables, which decreases the bias of the model as we have a more flexible model, but in turn our variance increases, making our models more variable.

From both cross validation and the testing data we select the linear regression model with interaction on age groups to be our best model. We include a plot of the predicted vs observed value from our final model.

![](https://res.cloudinary.com/kevinhe/image/upload/v1607216237/STAT%20350%20Project/predicted_aaijtt.png)

From this we're able to see that the predicted results are all scattered evenly around the y = x line.


#### Residual analysis
Our final task in this analysis is to ensure that our final model satisfies the assumptions for linear regression.

We first see if there is a linear relationship between at least one of the explanatory variables to the response by running a summary of the model. The resulting p-value from the significance of lienar regression is =< 0.05, so we conclude that the response is linearly related to at least one of the explanatory variables.

We now perform analysis on the residuals.
![](https://res.cloudinary.com/kevinhe/image/upload/v1607216516/STAT%20350%20Project/residuals_aiqgwj.png)

 By looking at the Residual vs Fitted plot, all the points are randomly scattered with a mean of 0 and no clear pattern, which suggests that the constant variance assumption is satisfied.

The Normal Q-Q plot, for the majority, follows a straight line with few points around the edges of both extremes deviating from the line. From this observation, the normality of the residuals is satisfied.

For the standardized residuals vs leverage plot, the points for leverage are scattered evenly around the same area with no points having a large leverage. As well, there exist a couple of points with standardized residuals beyond +/- 3, but since they do not have a high leverage point, they are not considered to be influential.

To double check that we do not have any influential points we use both Cook's Distance and diag(H) with standardized residuals, of which no influential points were identified.

Lastly, we use the Variance Inflation Factor to see if there exists an issue with multicollinearity in the model, and since all the values are ~ 1, we conclude that there is no issues with multicollinearity.

## Conclusion
  Overall, our final model has an R^2 value of 0.8891376 which is a great performance given the number of variables that were filtered from the data set. Running one last summary on our chosen model we are able to answer the questions posed in the beginning of the analysis. We first see that as the year increases there has been an improvement in the proportion of US adults that satisfies CDC guideline on muscle strengthening activity, although this change is quite marginal with a 0.38% increase per year from 2013 to 2015; this variable also poses as a limitation in our model which will be discussed later on. As well, we can see from the model that younger age groups are more likely to satisfy CDC guidelines on muscle strengthening activity, with about a 3% difference in proportion as age increases. Finally, we are able to conclude that the likelihood of satisfying CDC guidelines on muscle strengthening activities depends on the State/District of residence with states such as Hawaii, Alaska, Virginia, and Colorado having some of the largest increases for the intercept of the model, which is consistent with our exploratory analysis.

  Some of the limitations of the model include having the YearStart as one of the variables in our model as we do not know if the upwards trend will continuously be linear or if the increase will slow down as time goes on, forcing us to extrapolate for predictions outside of the year range 2013 - 2015. Moreover, including 51 levels of State/District and 6 levels of Age groups impacts our model as we have to fit a coefficient for each level which increases model complexity. In future studies, States/District could be grouped into 9 census regions or into Democratic and Republican controlled States/District to reduce their model complexity and to investigate further questions related to the data.

## References
- [Physical Activit Recommendations for Different Age Groups - CDC](https://www.cdc.gov/physicalactivity/basics/age-chart.html)
- [Lack of Physical Activity - CDC](https://www.cdc.gov/chronicdisease/resources/publications/factsheets/physical-activity.htm)
- [Physical Activity - CDC](https://www.cdc.gov/physicalactivity/basics/adults/index.htm#:~:text=Physical%20activities%20to%20strengthen%20your,addition%20to%20your%20aerobic%20activity.)

## Appendix
For the full code and analysis on this project [click here](https://github.com/kaishuun/Work-Out-Analysis/blob/main/Project.Rmd). For reproducibility purposes the seed `2928820` is set before any stochastic processes began.
