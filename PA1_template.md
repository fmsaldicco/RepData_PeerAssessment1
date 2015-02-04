# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
First we read the CSV file containing the observations:

```r
act <- read.csv("C:/tmp/activity.csv")
```

Then we have a look at the structure of the observations:

```r
str(act)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

And verify for the existence of **not available** (NA) data:

```r
anyNA(act$steps)
```

```
## [1] TRUE
```

```r
anyNA(act$date)
```

```
## [1] FALSE
```

```r
anyNA(act$interval)
```

```
## [1] FALSE
```
Therefore, NAs are present in the _steps_ variable.




## What is mean total number of steps taken per day?
In order to perform this analysis, a new data set is created having an aggregation of the steps taken each day.
The aggregation function will be the _sum_, as we want to sum all the steps taken in a day:

```r
aggbydate <- aggregate(act["steps"], by=act["date"], FUN="sum")
str(aggbydate)
```

```
## 'data.frame':	61 obs. of  2 variables:
##  $ date : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 2 3 4 5 6 7 8 9 10 ...
##  $ steps: int  NA 126 11352 12116 13294 15420 11015 NA 12811 9900 ...
```


This is a histogram showing the distribution of the total number of steps by day.

```r
hist(aggbydate$steps, xlab="number of steps by day",
     main="Total steps by day")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

It appears that the most common pattern is to walk between 10000 and 15000 steps by day.

Now, we can calculate mean and median, using the option na.rm=TRUE to ignore the NAs:

```r
mean(aggbydate$steps, na.rm=TRUE)
```

```
## [1] 10766.19
```

```r
median(aggbydate$steps, na.rm=TRUE)
```

```
## [1] 10765
```


## What is the average daily activity pattern?
In order to perform this analysis, a new data set is created having an aggregation of the steps taken by increment during the day.
The aggregation function will be the _mean_ in this case, as we want to have the average number of steps taken by increment. :

```r
aggbyinterval <- aggregate(act["steps"], by=act["interval"], FUN="mean", na.rm=TRUE)
```

The time series plot of this aggregate data sets is:

```r
plot(aggbyinterval, type="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-8-1.png) 

The interval with the maximum number of steps, on average, is:

```r
subset(aggbyinterval, steps==max(aggbyinterval$steps))
```

```
##     interval    steps
## 104      835 206.1698
```

## Imputing missing values
As stated above, we know that there are NAs in the _steps_ attribute.
Specifically, the number of NAs in the **activity** dataset is:

```r
sum(is.na(act$steps))
```

```
## [1] 2304
```
The strategy adopted to fill in the missing values is that of using the average number of steps for that interval.

```r
sum(is.na(act$steps))
```

```
## [1] 2304
```



## Are there differences in activity patterns between weekdays and weekends?
