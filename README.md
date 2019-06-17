
---
title: "Week Three"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Homework: Get means and counts for variables in a different data set.  What questions do you all have?

Generate data quickly
```{r}
set.seed(1234)
ordVar = c(1,2,3,4,5)
binVar = c(0,1)
set.seed(123)
datWeekTwo = data.frame(outcome1 = rnorm(100), outcome2 = rnorm(100), satisfaction = sample(ordVar, 100, replace = TRUE), gender = sample(binVar, 100, replace = TRUE))
datWeekTwo[1:10,1] = NA
datWeekTwo[11:15,2] = -99
datWeekThree = datWeekTwo
head(datWeekThree)
```
Quick review.  


Load data into R by first setting the working directory, then using the read.csv function.  Make sure header = TRUE to make the first row of data the variable names and if using na.strings make sure to include all NA indicators including NA in the list.
Quick new feature, we can get rid of missing data by using the na.omit function.  This function deletes any row with at least one missing value.  So if you are doing analyses with a large data set, you may want to subset only the data that you need for particular analyses and then use na.omit on that data set.

If we want to get means and sds, then we can library the descr package and use describe on the data set. If we want to get counts and percentages for categorical variables then we can library pretty R and use the describe.factor function.
```{r}
write.csv(datWeekThree, "datWeekThree.csv", row.names = FALSE)
datWeekThree = read.csv("datWeekThree.csv", header = TRUE, na.strings = c(NA, -99))
datWeekThree = na.omit(datWeekThree)
head(datWeekThree)

library(descr)
library(prettyR)

describe(datWeekThree)
describe.factor(datWeekThree$satisfaction)
describe.factor(datWeekThree$gender)
```
Sometimes you want to get cross tabs of different variables.  We can look at the means for males and females using the compmeans function and then round the results.

Also, the round function follows some different rules: https://stat.ethz.ch/R-manual/R-devel/library/base/html/Round.html
```{r}
round(compmeans(datWeekTwo$outcome1, datWeekTwo$gender),2)
```
Sometimes we want to subset the data.  For example, in the satisfaction variable, we can imagine it is on the following scale: 5 = strongly agree, 4 = agree, 3 = neutral, 2 = disagree, 1 = strongly disagree.  We may not be sure what to do with the neutral category so we may want to exclude those people.  We can use the subset function in R.  To subset the data where we exclude neutrals (i.e. 3's), we need two arguments, first is the dataset that we want to subset and second is the condition.  In this example, we want to exclude 3's so we say satisfaction!=3 to exclude the 3's.  In the other example below I show how to subset where you only include 5's using the == operator.  
```{r}
datWeekTwo_three = subset(datWeekTwo, satisfaction != 3)
datWeekTwo_three$satisfaction
datWeekTwoExample = subset(datWeekTwo, satisfaction == 5)
datWeekTwoExample$satisfaction
```
Our dates are usually month day year, so if we want to change them then we can use the format function. For the format, we want the format that the current date is in then R will change it for us.  For some reason you need to capitalize the Y in year not sure why.
```{r}
library(lubridate)
testDate = c("2/4/2018", "3/4/2018", "4/4/2018")
testDate
testDate = mdy(testDate)
testDate
testDate_feb = subset(testDate, testDate > "2018-02-04")
testDate_feb

### Now we want to in between some date and we want only march
testDate_mar = subset(testDate, testDate > "2018-02-04" & testDate < "2018-04-04")
testDate_mar
```

Just like in excel sometimes we want to use an if else statement.  If else statements allow us to change data based on some rules.   
```{r}
dat_recode = c("Male", "Female", "Another gender identity")
dat_recode_number = ifelse(dat_recode == "Male", 0, ifelse(dat_recode == "Female", 1, 2))
dat_recode_number
```
Now we are moving to a slighly more advanced function called apply.  Apply has other versions lapply, mapply, but we will focus on apply.  I think the best way to understand apply is through an example.  Let us say that we have a PHQ-9 with nine columns of data and we want to create a total score.  Let's run the data code below to create the fake data set.

Also, if you want to see the first six rows, you can use head(data set name)
```{r}
ordvar = c(0,1,2,3,4)
set.seed(124)
PHQ9 = data.frame(item1 = sample(ordvar, 100, replace = TRUE), item2 = sample(ordvar, 100, replace = TRUE), item3 = sample(ordvar, 100, replace = TRUE), item4 = sample(ordvar, 100, replace = TRUE), item5 = sample(ordvar, 100, replace = TRUE), item6 = sample(ordvar, 100, replace = TRUE), item7 = sample(ordvar, 100, replace = TRUE), item8 = sample(ordvar, 100, replace = TRUE), item9 = sample(ordvar, 100, replace = TRUE))
head(PHQ9)
```
Now we can use the apply function to sum across the nine rows.  First tell R which data set we want it to use, then we say 1, because we want it to sum across the rows (not columns), then we tell it what function we want it to use, which is the sum function.  We are creating a new variable PHQ9Total, which we then combine with the original PHQ9 data set giving us a PHQ9Total variable.
```{r}
PHQ9Total = apply(PHQ9, 1, sum)
head(PHQ9Total)
PHQ9$Total = PHQ9Total
PHQ9 = data.frame(PHQ9, PHQ9Total)
head(PHQ9)
apply(PHQ9, 2, mean)
```
Homework, try making a rule to subset your data and dicotimizing a variable with ifelse.




