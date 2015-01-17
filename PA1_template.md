# Reproducible Research: Peer Assessment 1

```r
library(lattice)
library(nnet)
```

## Loading and preprocessing the data

```r
temp <- tempfile()
download.file("https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip",temp,method="curl")
data <- read.csv(unz(temp, "activity.csv"), header=TRUE)
unlink(temp)
```
The required histogram of the number of steps is as follows: 

```r
hist(data$steps)
```

![plot of chunk unnamed-chunk-3](./PA1_template_files/figure-html/unnamed-chunk-3.png) 

## What is mean total number of steps taken per day?

```r
mean(data$steps, na.rm=TRUE)
```

```
## [1] 37.38
```

```r
median(data$steps,na.rm=TRUE)
```

```
## [1] 0
```
## What is the average daily activity pattern?
The "time series plot" of the 5-minute interval and the average number of steps taken, averaged across all days.

```r
aggdata<-aggregate(steps ~ interval, data, mean)
xyplot(aggdata$steps~aggdata$interval,type="l")
```

![plot of chunk unnamed-chunk-5](./PA1_template_files/figure-html/unnamed-chunk-5.png) 
The maximum value of steps is found at the following interval : 

```r
index<-which.is.max(aggdata$steps)
aggdata[index,]
```

```
##     interval steps
## 104      835 206.2
```

## Imputing missing values
The number of complete cases is 15264; the cases with missing data is therefore 2304.
I create a new dataframe, for imputing the missing data. 

```r
dataNoNa<-data
a<-which(is.na(data$steps))
for (index in 1:length(a)) {
        lineaggr<-which(aggdata$interval==dataNoNa$interval[a[index]])
        dataNoNa$steps[a[index]]<-aggdata$steps[lineaggr]        
}
```
Now, the histogram, mean and median of steps are the following: 

```r
hist(dataNoNa$steps)
```

![plot of chunk unnamed-chunk-8](./PA1_template_files/figure-html/unnamed-chunk-8.png) 

```r
mean(dataNoNa$steps)
```

```
## [1] 37.38
```

```r
median(dataNoNa$steps)
```

```
## [1] 0
```

## Are there differences in activity patterns between weekdays and weekends?
