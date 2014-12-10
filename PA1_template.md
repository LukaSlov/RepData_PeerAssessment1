# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
#setwd('~/Projects/Coursera/Reproductive Data/RepData_PeerAssessment1/')
data <- data.frame(read.csv(unzip('activity.zip', "activity.csv")))
#Aggregate number of steps by date
byDay <- aggregate(steps ~ date, data, sum)
#Average number of steps by interval
byInterval <- aggregate(steps ~ interval, data, sum)
byInterval$steps <- byInterval$steps/nrow(byInterval)
```


## What is mean total number of steps taken per day?

```r
hist(byDay$steps, breaks=10, main="Histogram of steps taken each day", xlab="Steps in a day")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
mean(byDay$steps)
```

```
## [1] 10766.19
```

```r
median(byDay$steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?

```r
plot(byInterval$interval, byInterval$steps, type='l', ylab="Average number of steps", xlab="5min interval of the day")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

```r
maxObservation <- which.max( byInterval$steps )
byInterval$steps[maxObservation]
```

```
## [1] 37.94097
```


## Imputing missing values

```r
#NA values
sum(is.na(data))
```

```
## [1] 2304
```

```r
dataNoNA <- data
for(i in 1:nrow(data)) {
  if(is.na(data[i,]$steps)){
    avgSteps = subset(byInterval, interval==data[i,]$interval)$steps
    dataNoNA[i,]$steps = avgSteps
  }
} 

byDayNoNA <- aggregate(steps ~ date, dataNoNA, sum)
hist(byDayNoNA$steps, breaks=10, main="Histogram of steps taken each day (NA replaced)", xlab="Steps in a day")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

```r
mean(byDayNoNA$steps)
```

```
## [1] 9614.069
```

```r
median(byDayNoNA$steps)
```

```
## [1] 10395
```


## Are there differences in activity patterns between weekdays and weekends?
