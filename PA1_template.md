---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

```r
data <- read.csv(file = "C:/Users/Shuai/Desktop/Jasmine R study/RepData_PeerAssessment1/activity.csv", header = TRUE)
```

## What is mean total number of steps taken per day?
### Calculate the total number of steps taken per day

```r
totalSteps <- aggregate(steps ~ date,data, FUN=sum)
```
### Make a histogram of the total number of steps taken each day

```r
hist(totalSteps$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
### Calculate and report the mean and median of the total number of steps taken per day

```r
meansteps <- mean(totalSteps$steps)
mediansteps <- median(totalSteps$steps)
```
## What is the average daily activity pattern?
###Make a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
fiveminsAVG <- aggregate(steps ~ interval, data, FUN=mean)
plot(x = fiveminsAVG$interval, y = fiveminsAVG$steps, type = "l")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->
### Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
maxInt <- fiveminsAVG[which.max(fiveminsAVG$steps),]
```

## Imputing missing values
### Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with \color{red}{\verb|NA|}NAs)

```r
totalNA <- sum(is.na(data$steps))
```
###Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

```r
rm(totalNA)
NAs <- which(is.na(data$steps))
mean <- rep(mean(data$steps, na.rm=TRUE), times=length(NAs))
```
###Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
data[NAs,"steps"] <- mean
sum(is.na(data$steps))
```

```
## [1] 0
```
###Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day.

```r
hist(totalSteps$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
meansteps1 <- mean(totalSteps$steps)
mediansteps1 <- median(totalSteps$steps)
```
###Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?
### Both stay the same.

## Are there differences in activity patterns between weekdays and weekends?
###Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

```r
data$weekday = weekdays(as.Date(data$date),abbreviate = FALSE)
data$type <- ifelse(data$weekday == "Saturday" | data$weekday == "Sunday", "Weekend", "Weekday")
```
###Make a panel plot containing a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). 

```r
library(lattice)
xyplot(steps ~ interval | type, layout = c(1, 2), data, type="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png)<!-- -->




