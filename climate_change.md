# Data Analytics in R

*portfolio of the data science projects in R*

## Project 3 : Climate Change 

*There have been many studies documenting that the average global temperature has been increasing over the last century. The consequences of a continued rise in global temperature will be dire. Rising sea levels and an increased frequency of extreme weather events will affect billions of people.*

*In this problem, we will attempt to study the relationship between average global temperature and several other factors*

The file climate_change.csv contains climate data from May 1983 to December 2008. The available variables include:

**Year:** the observation year.

**Month:** the observation month.

**Temp:** the difference in degrees Celsius between the average global temperature in that period and a reference value. This data comes from the Climatic Research Unit at the University of East Anglia.

**CO2, N2O, CH4, CFC.11, CFC.12:** atmospheric concentrations of carbon dioxide (CO2), nitrous oxide (N2O), methane  (CH4), trichlorofluoromethane (CCl3F; commonly referred to as CFC-11) and dichlorodifluoromethane (CCl2F2; commonly referred to as CFC-12), respectively. This data comes from the ESRL/NOAA Global Monitoring Division.

CO2, N2O and CH4 are expressed in ppmv (parts per million by volume  -- i.e., 397 ppmv of CO2 means that CO2 constitutes 397 millionths of the total volume of the atmosphere)
CFC.11 and CFC.12 are expressed in ppbv (parts per billion by volume). 

**Aerosols:** the mean stratospheric aerosol optical depth at 550 nm. This variable is linked to volcanoes, as volcanic eruptions result in new particles being added to the atmosphere, which affect how much of the sun's energy is reflected back into space. This data is from the Godard Institute for Space Studies at NASA.

**TSI:** the total solar irradiance (TSI) in W/m2 (the rate at which the sun's energy is deposited per unit area). Due to sunspots and other solar phenomena, the amount of energy that is given off by the sun varies substantially with time. This data is from the SOLARIS-HEPPA project website.

**MEI:** multivariate El Nino Southern Oscillation index (MEI), a measure of the strength of the El Nino/La Nina-Southern Oscillation (a weather effect in the Pacific Ocean that affects global temperatures). This data comes from the ESRL/NOAA Physical Sciences Division.

## Creating Our First Model : 

*We are interested in how changes in these variables affect future temperatures, as well as how well these variables explain temperature changes so far.*

To do this, first we need to read the dataset climate_change.csv into R.
```R
Climate=read.csv("climate_change.csv")
```

Then, we split the data into a training set, consisting of all the observations up to and including 2006, and a testing set consisting of the remaining year. A training set refers to the data that will be used to build the model (this is the data we give to the lm() function), and a testing set refers to the data we will use to test our predictive ability.

```R
train=subset(Climate, Climate$Year<=2006)
test=subset(Climate, Climate$Yea>2006)
```
Next, we build a linear regression model to predict the dependent variable Temp, using MEI, CO2, CH4, N2O, CFC.11, CFC.12, TSI, and Aerosols as independent variables (Year and Month should NOT be used in the model). 

we use the training set to build the model.
```R
Model=lm(Temp ~ MEI + CO2 + CH4 + N2O + CFC.11 + CFC.12 + TSI + Aerosols, data = train)
````

we can evaluate our model by checking the R2 (the "Multiple R-squared" value).

we get access to this value using the summary function.
```R
summary(lmModel)
```
Now let's check if the variables that we used to build our model are all significants or if exist some that not.

To do this we need to check the correlation between these variables and Temp. And we will consider a variable signficant only if the p-value is below 0.05. 

```R
cor(train)
```

After executing it we notice that CH4 and N2O are not really significant for our model.

## Understanding the Model : 

Current scientific opinion is that nitrous oxide and CFC-11 are greenhouse gases: gases that are able to trap heat from the sun and contribute to the heating of the Earth. However, the regression coefficients of both the N2O and CFC-11 variables are negative, indicating that increasing atmospheric concentrations of either of these two compounds is associated with lower global temperatures.

And the simplest explanation for this contradiction is : All of the gas concentration variables reflect human development - N2O and CFC.11 are correlated with other variables in the data set.

We can understand more our model by looking in the correlations between all the variables in the dataset.

## Simplifying the Model :

Given that the correlations are so high, let us focus on the N2O variable and build a model with only MEI, TSI, Aerosols and N2O as independent variables. 

We will always keep working with the training set to build the model.

```R
Model_simplified=lm(Temp ~ MEI+ N2O  + TSI + Aerosols, data = train)
summary(Model_simplified)
```

We can get again the coefficient for N2O and the model R-squared using summary(L).

We have observed that, for this problem, when we remove many variables the sign of N2O flips. The model has not lost a lot of explanatory power (the model R2 is 0.7261 compared to 0.7509 previously) despite removing many variables. As discussed in lecture, this type of behavior is typical when building a model where many of the independent variables are highly correlated with each other. In this particular problem many of the variables (CO2, CH4, N2O, CFC.11 and CFC.12) are highly correlated, since they are all driven by human industrial development.

## Automatically Building the Model :

We have many variables in this problem, and as we have seen above, dropping some from the model does not decrease model quality. R provides a function, step, that will automate the procedure of trying different combinations of variables to find a good compromise of model simplicity and R2. This trade-off is formalized by the Akaike information criterion (AIC) - it can be informally thought of as the quality of the model with a penalty for the number of variables in the model.

The step function has one argument - the name of the initial model. It returns a simplified model. Using the step function in R to derive a new model, with the full model as the initial model.

For more information about the step function, type ?step in R console.

the R2 value of the model produced by the step function: 0.7508

we obsereve also that the variable CH4 was eliminated.

It is interesting to note that the step function does not address the collinearity of the variables, except that adding highly correlated variables will not improve the R2 significantly. The consequence of this is that the step function will not necessarily produce a very interpretable model - just a model that has balanced quality and simplicity for a particular weighting of quality and simplicity (AIC).

## Testing on Unseen Data : 

We have developed an understanding of how well we can fit a linear regression to the training data, but does the model quality hold when applied to unseen data?

Using the model produced from the step function, calculate temperature predictions for the testing data set, using the predict function.

```R 
Model_auto=step(Model)
summary(Model_auto)
Temp_Pred=predict(Model_auto, newdata = test)
SSE = sum((Temp_Pred - test$Temp)^2)
SST = sum((mean(train$Temp) - test$Temp)^2)
R2 = 1 - SSE/SST
``` 

the testing set R2 : 0.6286

so our model did pretty good 









