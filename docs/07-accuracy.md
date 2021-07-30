# Classification accuracy {#accuracy}

To illustrate classification accuracy, here we compare two classifications of flapping migration. The first is using the inbuilt `classify_flap` function, the second is using pressure change. Both are then compared using `compare_classifications` and `compare_confusion_matrix`.




## Classify migratory flapping flight

Once a classification has been performed (here we use the example of a hoopoe, as it's migratory flight can be prediction using `classify_flap`)


```r
data("hoopoe")
# str(hoopoe)

# make sure the cropping period is in the correct date format
start = as.POSIXct("2016-07-01","%Y-%m-%d", tz="UTC")
end = as.POSIXct("2017-06-01","%Y-%m-%d", tz="UTC")

# Crop the data
PAM_data= create_crop(hoopoe,start,end)


# perform one classification using classify_flap
classification = classify_flap(dta = PAM_data$acceleration, period = 12, to_plot = FALSE)
```


This creates a timetable of migratory flight events which can be visualised using `classification$timetable`, as seen below:



<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-3)Migration timetable (first 10 rows)</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> start </th>
   <th style="text-align:center;"> end </th>
   <th style="text-align:center;"> Duration (h) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:center;"> 2016-08-06 20:20:00 </td>
   <td style="text-align:center;"> 2016-08-07 01:50:00 </td>
   <td style="text-align:center;"> 5.500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:center;"> 2016-08-07 19:40:00 </td>
   <td style="text-align:center;"> 2016-08-08 09:15:00 </td>
   <td style="text-align:center;"> 13.583333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:center;"> 2016-08-08 19:30:00 </td>
   <td style="text-align:center;"> 2016-08-09 04:10:00 </td>
   <td style="text-align:center;"> 8.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 6 </td>
   <td style="text-align:center;"> 2016-08-09 21:15:00 </td>
   <td style="text-align:center;"> 2016-08-10 01:30:00 </td>
   <td style="text-align:center;"> 4.250000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 7 </td>
   <td style="text-align:center;"> 2016-08-10 22:30:00 </td>
   <td style="text-align:center;"> 2016-08-10 23:50:00 </td>
   <td style="text-align:center;"> 1.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 8 </td>
   <td style="text-align:center;"> 2016-08-21 18:45:00 </td>
   <td style="text-align:center;"> 2016-08-22 04:15:00 </td>
   <td style="text-align:center;"> 9.500000 </td>
  </tr>
</tbody>
</table>




This classification is pretty accurate and we will use this as a reference dataset to compare with another classification: **high pressure change**. i.e. a high change in altitude.



## Setup the reference dataset

Because the second classification is done using pressure (30 minute data resolution) compared to this classification which was done using activity (5 minute resolution), the activity classification is set to the same resolution as pressure using the `create_merged_classification` function.


```r
# Put the classification in the same resolution as pressure
reference = create_merged_classification(from = classification$timetable$start,
                               to = classification$timetable$end,
                               classification = rep_len(1,length(classification$timetable$end)),
                               add_to = PAM_data$pressure,
                               missing = 0)

# Convert to categories
reference = ifelse(reference == 1, "Migration", "Other")
```

##  Setup the prediction data 

Hoopoes seems to perform large altitudinal changes during migratory flight, so we preform a very rough classification by specifying that any altitude change greater than 2 hPa is equivalent to a migratory flight (this is for illustrative purposes only, and should not be used as a definite classification method).


```r
# Perform another classification using pressure difference
prediction = c("Other",ifelse(abs(diff(PAM_data$pressure$obs))>2, "Migration", "Other"))
```

## Compare the two classifications

We can then compare the two classifications point by point using the `compare_classifications` function.


```r
# both classes have been converted to the same time intervals as pressure, so use those dates
date = PAM_data$pressure$date

# Combine the classifications into a dataframe
classifications = data.frame(reference= reference, # flapping classification
                             prediction = prediction) # pressure difference classification

class_comparison = compare_classifications(date=date,
                                classifications=classifications)
```

This puts both classifications side by side, and shows how many classifications provided each class, as well as the agreement between the two, as can be seen below.

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-7)Comparison of both classifications (first 10 rows)</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> reference </th>
   <th style="text-align:center;"> prediction </th>
   <th style="text-align:center;"> Migration </th>
   <th style="text-align:center;"> Other </th>
   <th style="text-align:center;"> agreement </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> Other </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
</tbody>
</table>


## Create a confusion Matrix

A confusion matrix uses predicted and reference points and estimate:

* **Errors in Commission** provide a measure of false negatives i.e. the number of points that were predicted to be part of a class that they were not (probability something was incorrectly prediction FN/(TP+FN)). 
* **Errors in Omission** provide a measure of false positives that were predicted to be in a different class from their actual class (probability that something was missed FP/(FP +TP). 
* **Producer Accuracy** or **Precision** provides a measure of how likely something was missed by the classification (probability that something was not missed TP/(TP + FP)). 
* **User Accuracy** or **Recall** represents the probability that a class was correctly prediction TP/(TP + FN).
* **Overall Accuracy** represents the probability that all classes were correctly prediction (TP+TN)/(TP+TN+FP+FN). 
* **Kappa Coefficient** measures the agreement between the classification and the truth ((TN+FP) (TN+FN) + (FN+TP) (FP+TP)) / (TP+FP+TN+FN)^2^



```r
mat = compare_confusion_matrix(reference, prediction)
```


<div style="border: 1px solid #ddd; padding: 5px; overflow-x: scroll; width:100%; "><table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-9)Confusion Matrix</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> Ref Other </th>
   <th style="text-align:center;"> Ref Migration </th>
   <th style="text-align:center;"> Row_Total </th>
   <th style="text-align:center;"> Commission_Error </th>
   <th style="text-align:center;"> Users_accuracy </th>
   <th style="text-align:center;"> Total_accuracy </th>
   <th style="text-align:center;"> Kappa_Coeff </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;width: 5cm; font-weight: bold;"> Pred Other </td>
   <td style="text-align:center;"> 2.990700e+04 </td>
   <td style="text-align:center;"> 104.0000000 </td>
   <td style="text-align:center;"> 30011 </td>
   <td style="text-align:center;"> 0.0034654 </td>
   <td style="text-align:center;"> 0.9965346 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm; font-weight: bold;"> Pred Migration </td>
   <td style="text-align:center;"> 8.000000e+01 </td>
   <td style="text-align:center;"> 726.0000000 </td>
   <td style="text-align:center;"> 806 </td>
   <td style="text-align:center;"> 0.0992556 </td>
   <td style="text-align:center;"> 0.9007444 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm; font-weight: bold;"> Col_Total </td>
   <td style="text-align:center;"> 2.998700e+04 </td>
   <td style="text-align:center;"> 830.0000000 </td>
   <td style="text-align:center;"> 30817 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm; font-weight: bold;"> Omission_Error </td>
   <td style="text-align:center;"> 2.667800e-03 </td>
   <td style="text-align:center;"> 0.1253012 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm; font-weight: bold;"> Producers_accuracy </td>
   <td style="text-align:center;"> 9.973322e-01 </td>
   <td style="text-align:center;"> 0.8746988 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm; font-weight: bold;"> Total_accuracy </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 0.9940293 </td>
   <td style="text-align:center;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm; font-weight: bold;"> Kappa_Coeff </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 0.9483213 </td>
  </tr>
</tbody>
</table></div>

## Overall accuracy

The total accuracy is 99.4% which is not bad. Most of the error comes from  the omission of some migration periods by the prediction i.e. there are periods where the bird is performing a migratory flight, but remains at the same altitude and are therefore missed by the classification. However, this is only for 1.25% of the points.
