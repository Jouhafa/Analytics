#Data Analytics in R
*portfolio of the data science projects in R*
##Project 1 : Analytical Detective
*Crime is an international concern, but it is documented and handled in very different ways in different countries. In the United States, violent crimes and property crimes are recorded by the Federal Bureau of Investigation (FBI). Additionally, each city documents crime, and some cities release data regarding crime rates. The city of Chicago, Illinois releases crimedata from 2001 onward online. Chicago is the third most populous city in the United States, with a population of over 2.7 million people. The city of Chicago is shown in the map below, with the state of Illinois highlighted in red.*
![alt text](https://courses.edx.org/assets/courseware/v1/1026b5e44d7529313194b5029a538061/asset-v1:MITx+15.071x+2T2020+type@asset+block/ChicagoMap.png)
*There are two main types of crimes: violent crimes, and property crimes. In this problem, we'll focus on one specific type of property crime, called "motor vehicle theft" (sometimes referred to as grand theft auto). This is the act of stealing, or attempting to steal, a car. In this problem, we'll use some basic data analysis in R to understand the motor vehicle thefts in Chicago.*
###1-Loading the Data
Reading Data in R :
```R
Motor = read.csv("mvtWeek1.csv") 
``` 
Size of the dataset :
191641 observations of 11 variables
Max value of the variable "ID" :
9181151
Minimum value of the variable "Beat" :
111
The number of crimes for which an arrest was made :
15536
The number of the crime in the location "ALLEY":
2308
###2- Dates conversion and processing :
In many datasets, like this one, you have a date field. Unfortunately, R does not automatically recognize entries that look like dates. We need to use a function in R to extract the date and time. Take a look at the first entry of Date (remember to use square brackets when looking at a certain entry of a variable).
Fomrat of the dates :
Month/Day/Year Hour:Minute
Now, let's convert these characters into a Date object in R using the function :
y = as.Date(strptime(x, "%m/%d/%y %H:%M"))
This converts the variable "Date" into a Date object in R, so we can easily exract the information of dates
Like the month and year of the median date in our dataset:
2006-05-21
We can also extract the month and the day of the week, and add these variables to our data frame
Now we can answer questions like :
- In which month did the fewest motor vehicle thefts occur?
February
On which weekday did the most motor vehicle thefts occur?
Friday


Which month has the largest number of motor vehicle thefts for which an arrest was made?
January
###3-Data Visualization :
Now, let's make some plots to help us better understand how crime has changed over time in Chicago.
First, let's make a histogram of the variable Date
![alt text](https://um6p-my.sharepoint.com/personal/zakarya_jouhafa_emines_um6p_ma/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fzakarya%5Fjouhafa%5Femines%5Fum6p%5Fma%2FDocuments%2FEMINES%2FAnalytics%20edge%2Fedx%5Fcourse%2FUnit1%2FhistMotor%2Epng&parent=%2Fpersonal%2Fzakarya%5Fjouhafa%5Femines%5Fum6p%5Fma%2FDocuments%2FEMINES%2FAnalytics%20edge%2Fedx%5Fcourse%2FUnit1)
This histogram will help us answer a lot of question, for instance :
In general, does it look like crime increases or decreases from 2002 - 2012?
 - Decreases 
In general, does it look like crime increases or decreases from 2005 - 2008?
 - Decreases 
In general, does it look like crime increases or decreases from 2009 - 2011?
 - Increases 
Now, let's see how arrests have changed over time.
To do this we plot a boxplot of the variable "Date", sorted by the variable "Arrest"
In a boxplot, the bold horizontal line is the median value of the data, the box shows the range of values between the first quartile and third quartile, and the whiskers (the dotted lines extending outside the box) show the minimum and maximum values, excluding any outliers (which are plotted as circles). Outliers are defined by first computing the difference between the first and third quartile values, or the height of the box. This number is called the Inter-Quartile Range (IQR). Any point that is greater than the third quartile plus the IQR or less than the first quartile minus the IQR is considered an outlier.
![alt text](https://um6p-my.sharepoint.com/personal/zakarya_jouhafa_emines_um6p_ma/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fzakarya%5Fjouhafa%5Femines%5Fum6p%5Fma%2FDocuments%2FEMINES%2FAnalytics%20edge%2Fedx%5Fcourse%2FUnit1%2FboxplotMotor%2Epng&parent=%2Fpersonal%2Fzakarya%5Fjouhafa%5Femines%5Fum6p%5Fma%2FDocuments%2FEMINES%2FAnalytics%20edge%2Fedx%5Fcourse%2FUnit1)
So thanks it this boxplot we can notice for example that there were more crimes for which arrests were made in the first half of the time period


Data visualization can also be done by calculating the porpotions 
Examples :
    The proportion for motor vehicle thefts in 2001 was an arrest made: 0.1041173
    The proportion of motor vehicle thefts in 2007 was an arrest made:  0.08487395
    The proportion of motor vehicle thefts in 2012 was an arrest made:  0.03902924
###4-Data Analytics :
Analyzing this data could be useful to the Chicago Police Department when deciding where to allocate resources. If they want to increase the number of arrests that are made for motor vehicle thefts, where should they focus their efforts?
We want to find the top five locations where motor vehicle thefts occur. If you create a table of the LocationDescription variable, it is unfortunately very hard to read since there are 78 different locations in the data set.
The top five locations for motor vehicle thefts :
- Street
- Alley
- Gas station 
- Driveway
- Parking lot
We observe that only in these 5 locations we find 177510 crime which make 92.6% of our dataset
the locations has a much higher arrest rate than the other locations :
Gas Station
Day of the week where the most motor vehicle thefts at gas stations happen:
Saturday
Day of the week where the most motor vehicle thefts at driveways happen:
Saturday