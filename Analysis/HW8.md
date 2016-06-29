# HW8
Vinh Le  
June 29, 2016  


#Introduction
This tutorial is based on the EDA Exercises in the book, Doing Data Science, which I highly recommend.

There are 31 data sets named nyt1.csv, nyt2.csv,…, nyt31.csv, which can be downloaded from GitHub.

Each csv represents one (simulated) days worth of ads shown and clicks recorded on the New York Times homepage in May 2012. Each row in the csv represents a single user.

There are five columns: Age, Gender (0=female, 1=male), Impressions, Clicks, and Signed_In status (0=not signed in, 1=signed in).

#Getting the Data
Create a data frame called data1 from the nyt1.csv file


```r
fileLocation <- "http://stat.columbia.edu/~rachel/datasets/nyt1.csv"
data1 <- read.csv(url(fileLocation))

# Use the section below if you would rather import the data from a local file
# fileLocation <- "~/path/for/the/file/nyt1.csv"
# data1 <- read.csv("fileLocation")
```
Take a look at the first 5 rows to get a feel for the data


```r
head(data1)
```

```
##   Age Gender Impressions Clicks Signed_In
## 1  36      0           3      0         1
## 2  73      1           3      0         1
## 3  30      0           3      0         1
## 4  49      1           3      0         1
## 5  47      1          11      0         1
## 6  47      0          11      1         1
```

You can get a overview of the structure of the data by using the str() function


```r
str(data1) # str stands for 'structure'
```

```
## 'data.frame':	458441 obs. of  5 variables:
##  $ Age        : int  36 73 30 49 47 47 0 46 16 52 ...
##  $ Gender     : int  0 1 0 1 1 0 0 0 0 0 ...
##  $ Impressions: int  3 3 3 3 11 11 7 5 3 4 ...
##  $ Clicks     : int  0 0 0 0 0 1 1 0 0 0 ...
##  $ Signed_In  : int  1 1 1 1 1 1 0 1 1 1 ...
```


```r
# str() returns information about each column such as name, data type, etc.
```
It could also be helpful to look at a summary of the data. This gives you useful information such as the number of rows in the object, and the mean, median, and range (min-max) of each column


```r
summary(data1)
```

```
##       Age             Gender       Impressions         Clicks       
##  Min.   :  0.00   Min.   :0.000   Min.   : 0.000   Min.   :0.00000  
##  1st Qu.:  0.00   1st Qu.:0.000   1st Qu.: 3.000   1st Qu.:0.00000  
##  Median : 31.00   Median :0.000   Median : 5.000   Median :0.00000  
##  Mean   : 29.48   Mean   :0.367   Mean   : 5.007   Mean   :0.09259  
##  3rd Qu.: 48.00   3rd Qu.:1.000   3rd Qu.: 6.000   3rd Qu.:0.00000  
##  Max.   :108.00   Max.   :1.000   Max.   :20.000   Max.   :4.00000  
##    Signed_In     
##  Min.   :0.0000  
##  1st Qu.:0.0000  
##  Median :1.0000  
##  Mean   :0.7009  
##  3rd Qu.:1.0000  
##  Max.   :1.0000
```

Since we’re exploring, it’s helpful to visualize the distribution of some of the columns, so we can see what we’re working with and if anything looks out of the ordinary


```r
# distribution of the Age column
hist(data1$Age, main="", xlab="Age")
```

![](HW8_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


```r
range(data1$Age)
```

```
## [1]   0 108
```


```r
# distribution of the Impressions column
hist(data1$Impressions, main="", xlab="# of Impressions")
```

![](HW8_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


```r
range(data1$Impressions)
```

```
## [1]  0 20
```


```r
# distribution of the Clicks column
hist(data1$Clicks, main="", xlab="# of Clicks")
```

![](HW8_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


```r
range(data1$Clicks)
```

```
## [1] 0 4
```

Did you notice that there is a disproportionate number of ages recorded as zero? This could skew our analysis, so we may need to account for that later.

#Task 1
create a new variable named Age_Group, that groups users into age categories “<18”, “18-24”, “25-34”, “35-44”, “45-54”, “55-64”, and “65+”

1.1) Cut the ages into the desired groups and add the groups to a new column in data1 called Age_Group. The ‘cut’ function creates a factor with levels


```r
data1$Age_Group <- cut(data1$Age, c(-Inf, 18, 24, 34, 44, 54, 64, Inf))

levels(data1$Age_Group) <- c("<18", "18-24", "25-34", "35-44", "45-54", "55-64", "65+")
# Name the levels of 'Age_Group' for readability
```
Take a look at the changes:


```r
head(data1)
```

```
##   Age Gender Impressions Clicks Signed_In Age_Group
## 1  36      0           3      0         1     35-44
## 2  73      1           3      0         1       65+
## 3  30      0           3      0         1     25-34
## 4  49      1           3      0         1     45-54
## 5  47      1          11      0         1     45-54
## 6  47      0          11      1         1     45-54
```

#Task 2
For a single day, plot the distributions of ‘number of impressions’ and ‘click-through-rate’ by Age_Group. (CTR = clicks/impressions).

2.1) Create a subset of data1 to exclude rows where there are no impressions (if there are no impressions, we assume there will be no clicks). Name the new object d1


```r
d1 <- subset(data1, Impressions>0)
```
2.2) Add a column to d1 called CTR containing the click-through-rate


```r
d1$CTR <- d1$Clicks/d1$Impressions

head(d1)
```

```
##   Age Gender Impressions Clicks Signed_In Age_Group        CTR
## 1  36      0           3      0         1     35-44 0.00000000
## 2  73      1           3      0         1       65+ 0.00000000
## 3  30      0           3      0         1     25-34 0.00000000
## 4  49      1           3      0         1     45-54 0.00000000
## 5  47      1          11      0         1     45-54 0.00000000
## 6  47      0          11      1         1     45-54 0.09090909
```

2.3) Plot the distribution of Impressions>0, grouped by Age_Group, using the ggplot2 package


```r
library(ggplot2) # used for visualizations
ggplot(subset(d1, Impressions>0), aes(x=Impressions, fill=Age_Group))+
    geom_histogram(binwidth=1)
```

![](HW8_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

2.4) Plot the distribution of CTR>0, grouped by Age_Group


```r
ggplot(subset(d1, CTR>0), aes(x=CTR, fill=Age_Group))+
    labs(title="Click-through rate by age group (05/01/2012)")+
    geom_histogram(binwidth=.025)
```

![](HW8_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

#Plot the distribution of CTR and Age Group

```r
ggplot(subset(d1, CTR>0), aes(x=CTR, fill=Age_Group))+
    labs(title="Click-through rate by age group (05/14/2012)")+
    geom_histogram(binwidth=.025)
```

![](HW8_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

