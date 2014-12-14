# Reproducible Research: Peer Assessment 1

## Loading and preprocessing the data

Load data using read.csv and assign to a variable so the expression is evaluated to unit and nothing is renderend by knitr.


```r
activities <- read.csv("activity.csv")
```


## What is mean total number of steps taken per day?


```r
stepsPerDay <- aggregate(steps ~ date, activities, sum)
```

1 - Visualize the histogram of the total number of steps taken each day
9717 277 103 117 183 263

```r
hist(stepsPerDay$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

2 - The mean number of steps taken per day is 1.0766189\times 10^{4} and the median total number of steps taken per day is 10765

## What is the average daily activity pattern?

1 - A time series of the 5-minute interval


```r
meanStepsPerInterval <- aggregate(steps ~ interval, activities, mean)
plot(meanStepsPerInterval$interval, meanStepsPerInterval$steps, type = "l")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

2 - Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps 835

## Imputing missing values

1 - The total number of missing values in the dataset is 2304

2 - We will use the mean steps per interval to replace NAs

3 - Create a new dataset


```r
newd <- activities
newd$steps = replace(newd$steps, is.na(newd$steps), meanStepsPerInterval$step)
```

4- Make a histogram


```r
newStepsPerDay <- aggregate(steps ~ date, newd, sum)
hist(newStepsPerDay$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png) 

The new mean number of steps taken per day is 1.0766189\times 10^{4} and the new median total number of steps taken per day is 1.0766189\times 10^{4}

Yes values differ from the first part of the assignment but not much.newd

## Are there differences in activity patterns between weekdays and weekends?

1 - Create the factor week with the two levels "weekday" and "weekend"


```r
newd$week <- ifelse(weekdays(as.Date(newd$date)) %in% c("Saturday", "Sunday"), "weekend", "weekday") 
```


```r
weekend <- newd[newd$week == "weekend",]
weekendStepsPerDay <- aggregate(steps ~ interval, weekend, sum)
plot(weekendStepsPerDay$interval, weekendStepsPerDay$steps, type = "l")
```

![](PA1_template_files/figure-html/unnamed-chunk-8-1.png) 


```r
weekday <- newd[newd$week == "weekday",]
weekdayStepsPerDay <- aggregate(steps ~ interval, weekday, sum)
plot(weekdayStepsPerDay$interval, weekdayStepsPerDay$steps, type = "l")
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png) 


