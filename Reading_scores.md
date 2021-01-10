# Data Analytics in R

*portfolio of the data science projects in R*

## Project 4 : Reading Scores 

*The Programme for International Student Assessment (PISA) is a test given every three years to 15-year-old students from around the world to evaluate their performance in mathematics, reading, and science. This test provides a quantitative way to compare the performance of students from different parts of the world. In this homework assignment, we will predict the reading scores of students from the United States of America on the 2009 PISA exam.*

*The datasets pisa2009train.csv and pisa2009test.csv contain information about the demographics and schools for American students taking the exam, derived from 2009 PISA Public-Use Data Files distributed by the United States National Center for Education Statistics (NCES). While the datasets are not supposed to contain identifying information about students taking the test, by using the data you are bound by the NCES data use agreement, which prohibits any attempt to determine the identity of any student in the datasets.*

*Each row in the datasets pisa2009train.csv and pisa2009test.csv represents one student taking the exam. The datasets have the following variables:*

**grade:** The grade in school of the student (most 15-year-olds in America are in 10th grade).

**male:** Whether the student is male (1/0)

**raceeth:** The race/ethnicity composite of the student

**preschool:** Whether the student attended preschool (1/0)

**expectBachelors:** Whether the student expects to obtain a bachelor's degree (1/0)

**motherHS:** Whether the student's mother completed high school (1/0)

**motherBachelors:** Whether the student's mother obtained a bachelor's degree (1/0)

**motherWork:** Whether the student's mother has part-time or full-time work (1/0)

**fatherHS:** Whether the student's father completed high school (1/0)

**fatherBachelors:** Whether the student's father obtained a bachelor's degree (1/0)

**fatherWork:** Whether the student's father has part-time or full-time work (1/0)

**selfBornUS:** Whether the student was born in the United States of America (1/0)

**motherBornUS:** Whether the student's mother was born in the United States of America (1/0)

**fatherBornUS:** Whether the student's father was born in the United States of America (1/0)

**englishAtHome:** Whether the student speaks English at home (1/0)

**computerForSchoolwork:** Whether the student has access to a computer for schoolwork (1/0)

**read30MinsADay:** Whether the student reads for pleasure for 30 minutes/day (1/0)

**minutesPerWeekEnglish:** The number of minutes per week the student spend in English class

**studentsInEnglish:** The number of students in this student's English class at school

**schoolHasLibrary:** Whether this student's school has a library (1/0)

**publicSchool:** Whether this student attends a public school (1/0)

**urban:** Whether this student's school is in an urban area (1/0)

**schoolSize:** The number of students in this student's school

**readingScore:** The student's reading score, on a 1000-point scale

##  Dataset size : 

*We start by loading the training and testing sets using the read.csv() function, and save them as variables with the names pisaTrain and pisaTest.*


```R
pisaTrain = read.csv("pisa2009train.csv")
pisaTest = read.csv("pisa2009test.csv")
```
So to get the size of the dataset we use the str() function.

in our case, for example if we want to know how many students are there in the training set : str(pisaTrain) or nrow(pisaTrain).

## Summarizing the dataset : 

At this stage, we would like to explore our dataset or simply have an overview about the variable.

For instance, we are intereseted in the average reading test score of males.

This can be known via the tapply() function.

The code for the function is as follows: 

```R
tapply(pisaTrain$readingScore, pisaTrain$male, mean)
```

the result will be : *483.5325*

## Locating missing values : 

Every dataset misses values that we call obviously the missing values. By locating those values we will be aware of the percentage of data missing and in which varibles parcticually. 

We can read which variables have missing values from summary(pisaTrain). Because most variables are collected from study participants via survey, it is expected that most questions will have at least one missing value.

We notice that most the variables miss some values

## Removing missing values : 

Linear regression discards observations with missing data, so we will remove all such observations from the training and testing sets. Later we will learn about imputation, which deals with missing data by filling in missing values with plausible information.

So we to remove the missing values we write : 
```R
pisaTrain = na.omit(pisaTrain)
pisaTest = na.omit(pisaTest)
````
And we check how many observations our sets still have using str().



## Factor variables : 

Factor variables are variables that take on a discrete set of values, like the "Region" variable in the WHO dataset from the second lecture of Unit 1. This is an unordered factor because there isn't any natural ordering between the levels. An ordered factor has a natural ordering between the levels (an example would be the classifications "large," "medium," and "small").

For example, in our case an *unordered factor with at least 3 levels* will be : receeth 

And an ordered factor with at least 3 levels will be : grade.

## Unordered factors in regression models :

To include unordered factors in a linear regression model, we define one level as the "reference level" and add a binary variable for each of the remaining levels. In this way, a factor with n levels is replaced by n-1 binary variables. The reference level is typically selected to be the most frequently occurring level in the dataset.

As an example, consider the unordered factor variable "color", with levels "red", "green", and "blue". If "green" were the reference level, then we would add binary variables "colorred" and "colorblue" to a linear regression problem. All red examples would have colorred=1 and colorblue=0. All blue examples would have colorred=0 and colorblue=1. All green examples would have colorred=0 and colorblue=0.

Now, consider the variable "raceeth" in our problem, which has levels "American Indian/Alaska Native", "Asian", "Black", "Hispanic", "More than one race", "Native Hawaiian/Other Pacific Islander", and "White". Because it is the most common in our population, we will select White as the reference level.

We create a binary variable for each level except the reference level, so we would create all these variables except for raceethWhite.

*Examples :*

- For a student who is Asian,raceethAsian will be set to 1 and all the binary variables of raceeth would be set to 0.

- For a student who is white, all the binary variables of raceeth would be set to 0.

##  Building the Model :

Because the race variable takes on text values, it was loaded as a factor variable when we read in the dataset with read.csv() -- you can see this when you run str(pisaTrain) or str(pisaTest). However, by default R selects the first level alphabetically ("American Indian/Alaska Native") as the reference level of our factor instead of the most common level ("White"). 

To set the reference level of the factor we use the relevel() function :

```R 
pisaTrain$raceeth = relevel(pisaTrain$raceeth, "White")
pisaTest$raceeth = relevel(pisaTest$raceeth, "White")
``` 
But before you need to check if the variable is a factor. if not you can convert it using the factor() function.

for example : 
```R 
pisaTrain$raceeth = factor(pisaTrain$raceeth)
``` 

Now, build a linear regression model (call it lmScore) using the training set to predict readingScore using all the remaining variables.

It would be time-consuming to type all the variables, but R provides the shorthand notation "readingScore ~ ." to mean "predict readingScore using all the other variables in the data frame." The period is used to replace listing out all of the independent variables. 

We can train the model with:
```R 
lmScore = lm(readingScore~., data=pisaTrain)
``` 

We can then read the training set R^2 from the "Multiple R-squared" value of summary(lmScore). *R^2 = 0.3251* .


## Computing the root-mean squared error of the model : 

The training-set RMSE can be computed by first computing the SSE:

```R 
SSE = sum(lmScore$residuals^2)
``` 
and then dividing by the number of observations and taking the square root:
```R 
RMSE = sqrt(SSE / nrow(pisaTrain))
``` 
A alternative way of getting this answer would be with the following command:
```R
sqrt(mean(lmScore$residuals^2)).
``` 









