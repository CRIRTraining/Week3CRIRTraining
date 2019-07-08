---
title: "Week Four"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Now we are going to review dates and subset.  Most dates are in the format m/d/y.  However, R does not understand this format.  R needs the format y/m/d.  To change the format into the correct form, you can change a date variable by using as.Date and putting in the current format of the date.  Then we can subset for the date range that we want.
```{r}
library(lubridate)
testDate = c("2/4/2018", "3/4/2018", "4/4/2018")
testDate
testDate = mdy(testDate)
testDate
testDate_1 = subset(testDate, testDate > "2018-02-04")
testDate_2 = subset(testDate, testDate > "2018-02-04" & testDate < "2018-04-04")
testDate_2
```
Now we are moving to a slighly more advanced functions ifelse and apply.  Apply has other versions lapply, mapply, but we will focus on apply.  I think the best way to understand apply is through an example.  Let us say that we have a PHQ-9 with nine columns of data and we want to create a total score.  Let's run the data code below to create the fake data set.

Also, if you want to see the first six rows, you can use head(data set name)
```{r}
ordvar = c(0,1,2,3)
set.seed(124)
PHQ9 = data.frame(item1 = sample(ordvar, 100, replace = TRUE), item2 = sample(ordvar, 100, replace = TRUE), item3 = sample(ordvar, 100, replace = TRUE), item4 = sample(ordvar, 100, replace = TRUE), item5 = sample(ordvar, 100, replace = TRUE), item6 = sample(ordvar, 100, replace = TRUE), item7 = sample(ordvar, 100, replace = TRUE), item8 = sample(ordvar, 100, replace = TRUE), item9 = sample(ordvar, 100, replace = TRUE))
head(PHQ9)
```
Just like in excel sometimes we want to use an if else statement.  If else statements allow us to change data based on some rules.  For example, in our data set we may want to create a binary variable from the satisfaction variable where we have all agree (strongly agree and agree) as 1 and all disagrees (strongly disagree and disagree) as zero.  We can use an ifelse statement to change the satisfaction variable.  
```{r}
head(PHQ9$item1, 10)
PHQ9$item1_dic = ifelse(PHQ9$item1 >=3, 1, 0)
head(PHQ9$item1_dic, 10)
PHQ9$item1_dic = NULL
head(PHQ9)
```

Now we can use the apply function to sum across the nine rows.  First tell R which data set we want it to use, then we say 1, because we want it to sum across the rows (not columns), then we tell it what function we want it to use, which is the sum function.  We are creating a new variable PHQ9Total, which we then combine with the original PHQ9 data set giving us a PHQ9Total variable.
```{r}
PHQ9$Total = apply(PHQ9, 1, sum)
head(PHQ9)
```
Now starting with simple graphs
```{r}
genderSamp = c(1,0)
GAD7Samp = c(7:49)
set.seed(123)
datWeekFour = cbind(PHQ9, gender = sample(genderSamp, 100, replace = TRUE), GAD7 = sample(GAD7Samp, 100, replace = TRUE))
head(datWeekFour)
```
Using ggplot2, which is the main graphical tool in R.  First we create the plot and which variables that we want using the ggplot function and telling R which dataset and then using the aes function, which variables in that dataset that we want to use.
```{r}
#install.packages("ggplot2")
library(ggplot2)
ggplot(datWeekFour, aes(GAD7, PHQ9Total))
```
Now we are ready to tell ggplot what kind of graph we want.  Let us do a scatter plot first.  To get a scatter plot, we will add geom_point() and for a line graph, we will add geom_line().
```{r}
ggplot(datWeekFour, aes(GAD7, PHQ9Total))+
  geom_point()

ggplot(datWeekFour, aes(GAD7, PHQ9Total))+
  geom_line()
```
What other graphics are you all interested in?
Are you all interested in online graphics: https://shiny.rstudio.com/gallery/
Or are you all more interested in statistics?
