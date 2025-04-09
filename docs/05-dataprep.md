# Preparing data for analysis {#dataprep}



There are multiple ways of preparing data for analysis. 

* `create_custom_interpolation`: The most simple is to merge the data together (at a specified time resolution) and interpolated or not.
* `create_rolling_window`: It's also possible to derive summary statistics using a rolling window, which progresses across the timeseries to make calculations. 
* `create_summary_statistics`: Where there is a particular pattern that need to be extracted from the data such as sustained pressure change or activity, this function derives summary statistics for these periods


## Merge sensor data together

Because data from different sensors are collected at different temporal resolutions (e.g. 5 minutes, 30 mintues or4 hours), `reducePAM` formats data to the same time intervals as a specified variable (e.g. pressure) by summarising finer resolution data (median, sum or skip) and interpolating (or not) lower resolution data. 


``` r
# Crop the data
start = as.POSIXct("2015-08-01","%Y-%m-%d", tz="UTC")
end = as.POSIXct("2016-06-21","%Y-%m-%d", tz="UTC")
PAM_data = create_crop(bee_eater, start, end)
```

### Interpolation

Format it for every 30 mins and interpolate data with larger intervals, and provide median for data with smaller intervals.


``` r
TOclassify = create_custom_interpolation(PAM_data , "pressure", interp = TRUE, summary="median")
```

<div style="border: 1px solid #ddd; padding: 5px; overflow-x: scroll; width:100%; "><table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-4)(\#tab:unnamed-chunk-4)A table of the first 10 rows of a reducePAM dataset.</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> date </th>
   <th style="text-align:center;"> pressure </th>
   <th style="text-align:center;"> light </th>
   <th style="text-align:center;"> pit </th>
   <th style="text-align:center;"> act </th>
   <th style="text-align:center;"> temperature </th>
   <th style="text-align:center;"> gX </th>
   <th style="text-align:center;"> gY </th>
   <th style="text-align:center;"> gZ </th>
   <th style="text-align:center;"> mX </th>
   <th style="text-align:center;"> mY </th>
   <th style="text-align:center;"> mZ </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 00:00:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 796.00 </td>
   <td style="text-align:center;"> -1993.000 </td>
   <td style="text-align:center;"> -4741 </td>
   <td style="text-align:center;"> -2016.000 </td>
   <td style="text-align:center;"> 11156 </td>
   <td style="text-align:center;"> 12528.0 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 00:30:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 854.75 </td>
   <td style="text-align:center;"> -1630.875 </td>
   <td style="text-align:center;"> -4661 </td>
   <td style="text-align:center;"> -2759.125 </td>
   <td style="text-align:center;"> 10517 </td>
   <td style="text-align:center;"> 11372.5 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 01:00:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 913.50 </td>
   <td style="text-align:center;"> -1268.750 </td>
   <td style="text-align:center;"> -4581 </td>
   <td style="text-align:center;"> -3502.250 </td>
   <td style="text-align:center;"> 9878 </td>
   <td style="text-align:center;"> 10217.0 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 01:30:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 972.25 </td>
   <td style="text-align:center;"> -906.625 </td>
   <td style="text-align:center;"> -4501 </td>
   <td style="text-align:center;"> -4245.375 </td>
   <td style="text-align:center;"> 9239 </td>
   <td style="text-align:center;"> 9061.5 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 02:00:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1031.00 </td>
   <td style="text-align:center;"> -544.500 </td>
   <td style="text-align:center;"> -4421 </td>
   <td style="text-align:center;"> -4988.500 </td>
   <td style="text-align:center;"> 8600 </td>
   <td style="text-align:center;"> 7906.0 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 02:30:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1089.75 </td>
   <td style="text-align:center;"> -182.375 </td>
   <td style="text-align:center;"> -4341 </td>
   <td style="text-align:center;"> -5731.625 </td>
   <td style="text-align:center;"> 7961 </td>
   <td style="text-align:center;"> 6750.5 </td>
  </tr>
</tbody>
</table></div>

###  No interpolation

Format it for every 5 minutes and don't interpolate anything


``` r
TOclassify = create_custom_interpolation(PAM_data , "acceleration", interp = FALSE)
```


## Rolling window

Interpolation is not always advisable (especially linear), and another alternative for formatting data for analysis is to use a rolling window with `create_rolling_window`, which progresses across all the timeseries and creates summary statistics for the data contained within that window of a certain time. 

Derived variables include:

* **median** : Median
* **sd** : Standard deviation
* **sum** : Cumulative sum of values 
* **min** : Minimum
* **max** : Maximum
* **range** : Range (i.e. maximum - minimum)
* **cumu_diff** : Cumulative difference (i.e. sum of absolute differences)

### Interpolation

Create a 2h window with summary statistics every 15 minutes. Because sensors such as the magnetometer record every 4 hours, we can avoid spaces in the dataset by interpolating between points (linearly) and then calculating summary statistics for these interpolated points.


``` r
TOclassify = create_rolling_window(PAM_data,
                                   resolution_out = 15 ,
                                   window = 120)
```

<div style="border: 1px solid #ddd; padding: 5px; overflow-x: scroll; width:100%; "><table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-7)(\#tab:unnamed-chunk-7)A table of the first 10 rows of a reducePAM dataset.</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> date </th>
   <th style="text-align:center;"> pressure </th>
   <th style="text-align:center;"> light </th>
   <th style="text-align:center;"> pit </th>
   <th style="text-align:center;"> act </th>
   <th style="text-align:center;"> temperature </th>
   <th style="text-align:center;"> gX </th>
   <th style="text-align:center;"> gY </th>
   <th style="text-align:center;"> gZ </th>
   <th style="text-align:center;"> mX </th>
   <th style="text-align:center;"> mY </th>
   <th style="text-align:center;"> mZ </th>
   <th style="text-align:center;"> median_pressure </th>
   <th style="text-align:center;"> median_light </th>
   <th style="text-align:center;"> median_pit </th>
   <th style="text-align:center;"> median_act </th>
   <th style="text-align:center;"> median_temperature </th>
   <th style="text-align:center;"> median_gX </th>
   <th style="text-align:center;"> median_gY </th>
   <th style="text-align:center;"> median_gZ </th>
   <th style="text-align:center;"> median_mX </th>
   <th style="text-align:center;"> median_mY </th>
   <th style="text-align:center;"> median_mZ </th>
   <th style="text-align:center;"> sd_pressure </th>
   <th style="text-align:center;"> sd_light </th>
   <th style="text-align:center;"> sd_pit </th>
   <th style="text-align:center;"> sd_act </th>
   <th style="text-align:center;"> sd_temperature </th>
   <th style="text-align:center;"> sd_gX </th>
   <th style="text-align:center;"> sd_gY </th>
   <th style="text-align:center;"> sd_gZ </th>
   <th style="text-align:center;"> sd_mX </th>
   <th style="text-align:center;"> sd_mY </th>
   <th style="text-align:center;"> sd_mZ </th>
   <th style="text-align:center;"> sum_pressure </th>
   <th style="text-align:center;"> sum_light </th>
   <th style="text-align:center;"> sum_pit </th>
   <th style="text-align:center;"> sum_act </th>
   <th style="text-align:center;"> sum_temperature </th>
   <th style="text-align:center;"> sum_gX </th>
   <th style="text-align:center;"> sum_gY </th>
   <th style="text-align:center;"> sum_gZ </th>
   <th style="text-align:center;"> sum_mX </th>
   <th style="text-align:center;"> sum_mY </th>
   <th style="text-align:center;"> sum_mZ </th>
   <th style="text-align:center;"> min_pressure </th>
   <th style="text-align:center;"> min_light </th>
   <th style="text-align:center;"> min_pit </th>
   <th style="text-align:center;"> min_act </th>
   <th style="text-align:center;"> min_temperature </th>
   <th style="text-align:center;"> min_gX </th>
   <th style="text-align:center;"> min_gY </th>
   <th style="text-align:center;"> min_gZ </th>
   <th style="text-align:center;"> min_mX </th>
   <th style="text-align:center;"> min_mY </th>
   <th style="text-align:center;"> min_mZ </th>
   <th style="text-align:center;"> max_pressure </th>
   <th style="text-align:center;"> max_light </th>
   <th style="text-align:center;"> max_pit </th>
   <th style="text-align:center;"> max_act </th>
   <th style="text-align:center;"> max_temperature </th>
   <th style="text-align:center;"> max_gX </th>
   <th style="text-align:center;"> max_gY </th>
   <th style="text-align:center;"> max_gZ </th>
   <th style="text-align:center;"> max_mX </th>
   <th style="text-align:center;"> max_mY </th>
   <th style="text-align:center;"> max_mZ </th>
   <th style="text-align:center;"> cumu_diff_pressure </th>
   <th style="text-align:center;"> cumu_diff_light </th>
   <th style="text-align:center;"> cumu_diff_pit </th>
   <th style="text-align:center;"> cumu_diff_act </th>
   <th style="text-align:center;"> cumu_diff_temperature </th>
   <th style="text-align:center;"> cumu_diff_gX </th>
   <th style="text-align:center;"> cumu_diff_gY </th>
   <th style="text-align:center;"> cumu_diff_gZ </th>
   <th style="text-align:center;"> cumu_diff_mX </th>
   <th style="text-align:center;"> cumu_diff_mY </th>
   <th style="text-align:center;"> cumu_diff_mZ </th>
   <th style="text-align:center;"> range_pressure </th>
   <th style="text-align:center;"> range_light </th>
   <th style="text-align:center;"> range_pit </th>
   <th style="text-align:center;"> range_act </th>
   <th style="text-align:center;"> range_temperature </th>
   <th style="text-align:center;"> range_gX </th>
   <th style="text-align:center;"> range_gY </th>
   <th style="text-align:center;"> range_gZ </th>
   <th style="text-align:center;"> range_mX </th>
   <th style="text-align:center;"> range_mY </th>
   <th style="text-align:center;"> range_mZ </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 00:45:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 884.125 </td>
   <td style="text-align:center;"> -1449.8125 </td>
   <td style="text-align:center;"> -4621 </td>
   <td style="text-align:center;"> -3130.688 </td>
   <td style="text-align:center;"> 10197.5 </td>
   <td style="text-align:center;"> 10794.75 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 898.8125 </td>
   <td style="text-align:center;"> -1359.2812 </td>
   <td style="text-align:center;"> -4601 </td>
   <td style="text-align:center;"> -3316.469 </td>
   <td style="text-align:center;"> 10037.75 </td>
   <td style="text-align:center;"> 10505.875 </td>
   <td style="text-align:center;"> 0.0000000 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0.3535534 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 71.95376 </td>
   <td style="text-align:center;"> 443.5107 </td>
   <td style="text-align:center;"> 97.97959 </td>
   <td style="text-align:center;"> 910.1385 </td>
   <td style="text-align:center;"> 782.612 </td>
   <td style="text-align:center;"> 1415.193 </td>
   <td style="text-align:center;"> 8032.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 191 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 264 </td>
   <td style="text-align:center;"> 7190.5 </td>
   <td style="text-align:center;"> -10874.25 </td>
   <td style="text-align:center;"> -36808 </td>
   <td style="text-align:center;"> -26531.75 </td>
   <td style="text-align:center;"> 80302 </td>
   <td style="text-align:center;"> 84047 </td>
   <td style="text-align:center;"> 1004.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 23 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 796.000 </td>
   <td style="text-align:center;"> -1993.000 </td>
   <td style="text-align:center;"> -4741 </td>
   <td style="text-align:center;"> -4616.938 </td>
   <td style="text-align:center;"> 8919.5 </td>
   <td style="text-align:center;"> 8483.75 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1001.625 </td>
   <td style="text-align:center;"> -725.5625 </td>
   <td style="text-align:center;"> -4461 </td>
   <td style="text-align:center;"> -2016.000 </td>
   <td style="text-align:center;"> 11156.0 </td>
   <td style="text-align:center;"> 12528.00 </td>
   <td style="text-align:center;"> 0.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
   <td style="text-align:center;"> 0.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 01:00:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 23 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 913.500 </td>
   <td style="text-align:center;"> -1268.7500 </td>
   <td style="text-align:center;"> -4581 </td>
   <td style="text-align:center;"> -3502.250 </td>
   <td style="text-align:center;"> 9878.0 </td>
   <td style="text-align:center;"> 10217.00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 928.1875 </td>
   <td style="text-align:center;"> -1178.2188 </td>
   <td style="text-align:center;"> -4561 </td>
   <td style="text-align:center;"> -3688.031 </td>
   <td style="text-align:center;"> 9718.25 </td>
   <td style="text-align:center;"> 9928.125 </td>
   <td style="text-align:center;"> 0.0000000 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0.3535534 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 71.95376 </td>
   <td style="text-align:center;"> 443.5107 </td>
   <td style="text-align:center;"> 97.97959 </td>
   <td style="text-align:center;"> 910.1385 </td>
   <td style="text-align:center;"> 782.612 </td>
   <td style="text-align:center;"> 1415.193 </td>
   <td style="text-align:center;"> 8032.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 191 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 264 </td>
   <td style="text-align:center;"> 7425.5 </td>
   <td style="text-align:center;"> -9425.75 </td>
   <td style="text-align:center;"> -36488 </td>
   <td style="text-align:center;"> -29504.25 </td>
   <td style="text-align:center;"> 77746 </td>
   <td style="text-align:center;"> 79425 </td>
   <td style="text-align:center;"> 1004.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 23 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 825.375 </td>
   <td style="text-align:center;"> -1811.938 </td>
   <td style="text-align:center;"> -4701 </td>
   <td style="text-align:center;"> -4988.500 </td>
   <td style="text-align:center;"> 8600.0 </td>
   <td style="text-align:center;"> 7906.00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1031.000 </td>
   <td style="text-align:center;"> -544.5000 </td>
   <td style="text-align:center;"> -4421 </td>
   <td style="text-align:center;"> -2387.562 </td>
   <td style="text-align:center;"> 10836.5 </td>
   <td style="text-align:center;"> 11950.25 </td>
   <td style="text-align:center;"> 0.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
   <td style="text-align:center;"> 0.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 01:15:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 942.875 </td>
   <td style="text-align:center;"> -1087.6875 </td>
   <td style="text-align:center;"> -4541 </td>
   <td style="text-align:center;"> -3873.812 </td>
   <td style="text-align:center;"> 9558.5 </td>
   <td style="text-align:center;"> 9639.25 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 957.5625 </td>
   <td style="text-align:center;"> -997.1562 </td>
   <td style="text-align:center;"> -4521 </td>
   <td style="text-align:center;"> -4059.594 </td>
   <td style="text-align:center;"> 9398.75 </td>
   <td style="text-align:center;"> 9350.375 </td>
   <td style="text-align:center;"> 0.0000000 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0.3535534 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 71.95376 </td>
   <td style="text-align:center;"> 443.5107 </td>
   <td style="text-align:center;"> 97.97959 </td>
   <td style="text-align:center;"> 910.1385 </td>
   <td style="text-align:center;"> 782.612 </td>
   <td style="text-align:center;"> 1415.193 </td>
   <td style="text-align:center;"> 8032.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 191 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 264 </td>
   <td style="text-align:center;"> 7660.5 </td>
   <td style="text-align:center;"> -7977.25 </td>
   <td style="text-align:center;"> -36168 </td>
   <td style="text-align:center;"> -32476.75 </td>
   <td style="text-align:center;"> 75190 </td>
   <td style="text-align:center;"> 74803 </td>
   <td style="text-align:center;"> 1004.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 23 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 854.750 </td>
   <td style="text-align:center;"> -1630.875 </td>
   <td style="text-align:center;"> -4661 </td>
   <td style="text-align:center;"> -5360.062 </td>
   <td style="text-align:center;"> 8280.5 </td>
   <td style="text-align:center;"> 7328.25 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1060.375 </td>
   <td style="text-align:center;"> -363.4375 </td>
   <td style="text-align:center;"> -4381 </td>
   <td style="text-align:center;"> -2759.125 </td>
   <td style="text-align:center;"> 10517.0 </td>
   <td style="text-align:center;"> 11372.50 </td>
   <td style="text-align:center;"> 0.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
   <td style="text-align:center;"> 0.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 01:30:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 972.250 </td>
   <td style="text-align:center;"> -906.6250 </td>
   <td style="text-align:center;"> -4501 </td>
   <td style="text-align:center;"> -4245.375 </td>
   <td style="text-align:center;"> 9239.0 </td>
   <td style="text-align:center;"> 9061.50 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 986.9375 </td>
   <td style="text-align:center;"> -816.0938 </td>
   <td style="text-align:center;"> -4481 </td>
   <td style="text-align:center;"> -4431.156 </td>
   <td style="text-align:center;"> 9079.25 </td>
   <td style="text-align:center;"> 8772.625 </td>
   <td style="text-align:center;"> 0.0000000 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0.3535534 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 71.95376 </td>
   <td style="text-align:center;"> 443.5107 </td>
   <td style="text-align:center;"> 97.97959 </td>
   <td style="text-align:center;"> 910.1385 </td>
   <td style="text-align:center;"> 782.612 </td>
   <td style="text-align:center;"> 1415.193 </td>
   <td style="text-align:center;"> 8032.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 191 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 264 </td>
   <td style="text-align:center;"> 7895.5 </td>
   <td style="text-align:center;"> -6528.75 </td>
   <td style="text-align:center;"> -35848 </td>
   <td style="text-align:center;"> -35449.25 </td>
   <td style="text-align:center;"> 72634 </td>
   <td style="text-align:center;"> 70181 </td>
   <td style="text-align:center;"> 1004.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 23 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 884.125 </td>
   <td style="text-align:center;"> -1449.812 </td>
   <td style="text-align:center;"> -4621 </td>
   <td style="text-align:center;"> -5731.625 </td>
   <td style="text-align:center;"> 7961.0 </td>
   <td style="text-align:center;"> 6750.50 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1089.750 </td>
   <td style="text-align:center;"> -182.3750 </td>
   <td style="text-align:center;"> -4341 </td>
   <td style="text-align:center;"> -3130.688 </td>
   <td style="text-align:center;"> 10197.5 </td>
   <td style="text-align:center;"> 10794.75 </td>
   <td style="text-align:center;"> 0.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
   <td style="text-align:center;"> 0.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 01:45:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1001.625 </td>
   <td style="text-align:center;"> -725.5625 </td>
   <td style="text-align:center;"> -4461 </td>
   <td style="text-align:center;"> -4616.938 </td>
   <td style="text-align:center;"> 8919.5 </td>
   <td style="text-align:center;"> 8483.75 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1016.3125 </td>
   <td style="text-align:center;"> -635.0312 </td>
   <td style="text-align:center;"> -4441 </td>
   <td style="text-align:center;"> -4802.719 </td>
   <td style="text-align:center;"> 8759.75 </td>
   <td style="text-align:center;"> 8194.875 </td>
   <td style="text-align:center;"> 0.1767767 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0.3535534 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 71.95376 </td>
   <td style="text-align:center;"> 443.5107 </td>
   <td style="text-align:center;"> 97.97959 </td>
   <td style="text-align:center;"> 910.1385 </td>
   <td style="text-align:center;"> 782.612 </td>
   <td style="text-align:center;"> 1415.193 </td>
   <td style="text-align:center;"> 8031.5 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 191 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 264 </td>
   <td style="text-align:center;"> 8130.5 </td>
   <td style="text-align:center;"> -5080.25 </td>
   <td style="text-align:center;"> -35528 </td>
   <td style="text-align:center;"> -38421.75 </td>
   <td style="text-align:center;"> 70078 </td>
   <td style="text-align:center;"> 65559 </td>
   <td style="text-align:center;"> 1003.5 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 23 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 913.500 </td>
   <td style="text-align:center;"> -1268.750 </td>
   <td style="text-align:center;"> -4581 </td>
   <td style="text-align:center;"> -6103.188 </td>
   <td style="text-align:center;"> 7641.5 </td>
   <td style="text-align:center;"> 6172.75 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1119.125 </td>
   <td style="text-align:center;"> -1.3125 </td>
   <td style="text-align:center;"> -4301 </td>
   <td style="text-align:center;"> -3502.250 </td>
   <td style="text-align:center;"> 9878.0 </td>
   <td style="text-align:center;"> 10217.00 </td>
   <td style="text-align:center;"> 0.5 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
   <td style="text-align:center;"> 0.5 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 02:00:00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1031.000 </td>
   <td style="text-align:center;"> -544.5000 </td>
   <td style="text-align:center;"> -4421 </td>
   <td style="text-align:center;"> -4988.500 </td>
   <td style="text-align:center;"> 8600.0 </td>
   <td style="text-align:center;"> 7906.00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1045.6875 </td>
   <td style="text-align:center;"> -453.9688 </td>
   <td style="text-align:center;"> -4401 </td>
   <td style="text-align:center;"> -5174.281 </td>
   <td style="text-align:center;"> 8440.25 </td>
   <td style="text-align:center;"> 7617.125 </td>
   <td style="text-align:center;"> 0.3720119 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0.0000000 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 71.95376 </td>
   <td style="text-align:center;"> 443.5107 </td>
   <td style="text-align:center;"> 97.97959 </td>
   <td style="text-align:center;"> 910.1385 </td>
   <td style="text-align:center;"> 782.612 </td>
   <td style="text-align:center;"> 1415.193 </td>
   <td style="text-align:center;"> 8030.5 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 192 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 264 </td>
   <td style="text-align:center;"> 8365.5 </td>
   <td style="text-align:center;"> -3631.75 </td>
   <td style="text-align:center;"> -35208 </td>
   <td style="text-align:center;"> -41394.25 </td>
   <td style="text-align:center;"> 67522 </td>
   <td style="text-align:center;"> 60937 </td>
   <td style="text-align:center;"> 1003.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 942.875 </td>
   <td style="text-align:center;"> -1087.688 </td>
   <td style="text-align:center;"> -4541 </td>
   <td style="text-align:center;"> -6474.750 </td>
   <td style="text-align:center;"> 7322.0 </td>
   <td style="text-align:center;"> 5595.00 </td>
   <td style="text-align:center;"> 1004 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 33 </td>
   <td style="text-align:center;"> 1148.500 </td>
   <td style="text-align:center;"> 179.7500 </td>
   <td style="text-align:center;"> -4261 </td>
   <td style="text-align:center;"> -3873.812 </td>
   <td style="text-align:center;"> 9558.5 </td>
   <td style="text-align:center;"> 9639.25 </td>
   <td style="text-align:center;"> 1.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
   <td style="text-align:center;"> 1.0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 205.625 </td>
   <td style="text-align:center;"> 1267.438 </td>
   <td style="text-align:center;"> 280 </td>
   <td style="text-align:center;"> 2600.938 </td>
   <td style="text-align:center;"> 2236.5 </td>
   <td style="text-align:center;"> 4044.25 </td>
  </tr>
</tbody>
</table></div>

### No interpolation

However, there are many assumpations made with assumptions (i.e. is the data truly linear). One option is either to increase the window to be larger than the greatest data resolution (in this case more than 4 hours). Another is to simply leave the NAs in the data using `interp = FALSE`


``` r
TOclassify = create_rolling_window(PAM_data,
                                   resolution_out = 15,
                                   window = 120,
                                   interp = FALSE)
```

## Extracting statistics for specific data patterns

If working with bird data, pamlr offers some predefined functions for classifying behaviour. 

* Flight bouts can be characterised by:

    + continuous high activity which can be extracted from the data using `create_summary_statistics( ... ,method = "flap")` 
    + endurance activity using `create_summary_statistics( ... ,method = "endurance")`
    + a pressure change greater than the background pressure changes due to weather using `create_summary_statistics( ... ,method = "pressure")`
    + a period of continuous light using `create_summary_statistics( ... ,method = "light")`
  
* Incubation bouts can be characterised by: 

    + periods of darkness using `create_summary_statistics( ... ,method = "darkness")`
    + periods of resting using `create_summary_statistics( ... ,method = "rest")`

The old _twilightCalc_ function (now deprecated) from GeoLight has been added into pamlr for ease.However, for accurate twilight calculations, please refer to the more updated TwGeos function preProcessLight here: https://geolocationmanual.vogelwarte.ch/twilight.html 


``` r
twl = twilightCalc(PAM_data$light$date, PAM_data$light$obs, 
                   LightThreshold = 2, ask = FALSE)


TOclassify = create_summary_statistics(dta = PAM_data,
                                       method= "flap",
                                       twl = twl)
```

<div style="border: 1px solid #ddd; padding: 5px; overflow-x: scroll; width:100%; "><table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-10)(\#tab:unnamed-chunk-10)A table of the first 10 rows of a reducePAM dataset.</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> date </th>
   <th style="text-align:center;"> start </th>
   <th style="text-align:center;"> end </th>
   <th style="text-align:center;"> duration </th>
   <th style="text-align:center;"> total_daily_duration </th>
   <th style="text-align:center;"> total_daily_event_number </th>
   <th style="text-align:center;"> cum_pressure_change </th>
   <th style="text-align:center;"> cum_altitude_change </th>
   <th style="text-align:center;"> cum_altitude_up </th>
   <th style="text-align:center;"> total_daily_P_change </th>
   <th style="text-align:center;"> P_dep_arr </th>
   <th style="text-align:center;"> pressure_range </th>
   <th style="text-align:center;"> altitude_range </th>
   <th style="text-align:center;"> mean_night_P </th>
   <th style="text-align:center;"> sd_night_P </th>
   <th style="text-align:center;"> mean_nextnight_P </th>
   <th style="text-align:center;"> sd_nextnight_P </th>
   <th style="text-align:center;"> night_P_diff </th>
   <th style="text-align:center;"> median_activity </th>
   <th style="text-align:center;"> sum_activity </th>
   <th style="text-align:center;"> prop_resting </th>
   <th style="text-align:center;"> prop_active </th>
   <th style="text-align:center;"> mean_night_act </th>
   <th style="text-align:center;"> sd_night_act </th>
   <th style="text-align:center;"> sum_night_act </th>
   <th style="text-align:center;"> mean_nextnight_act </th>
   <th style="text-align:center;"> sd_nextnight_act </th>
   <th style="text-align:center;"> sum_nextnight_act </th>
   <th style="text-align:center;"> night_act_diff </th>
   <th style="text-align:center;"> median_pitch </th>
   <th style="text-align:center;"> sd_pitch </th>
   <th style="text-align:center;"> median_light </th>
   <th style="text-align:center;"> nightime </th>
   <th style="text-align:center;"> median_gX </th>
   <th style="text-align:center;"> median_gY </th>
   <th style="text-align:center;"> median_gZ </th>
   <th style="text-align:center;"> median_mX </th>
   <th style="text-align:center;"> median_mY </th>
   <th style="text-align:center;"> median_mZ </th>
   <th style="text-align:center;"> median_temp </th>
   <th style="text-align:center;"> sd_temp </th>
   <th style="text-align:center;"> cum_temp_change </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 </td>
   <td style="text-align:center;"> 2015-08-01 12:05:00 </td>
   <td style="text-align:center;"> 2015-08-01 12:30:00 </td>
   <td style="text-align:center;"> 0.4166667 </td>
   <td style="text-align:center;"> 0.9166667 </td>
   <td style="text-align:center;"> 4 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1001.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 1004.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 3.00 </td>
   <td style="text-align:center;"> 16 </td>
   <td style="text-align:center;"> 100 </td>
   <td style="text-align:center;"> 0.1666667 </td>
   <td style="text-align:center;"> 0.8333333 </td>
   <td style="text-align:center;"> 0.0210526 </td>
   <td style="text-align:center;"> 0.1443214 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0.0107527 </td>
   <td style="text-align:center;"> 0.1036952 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0.0102999 </td>
   <td style="text-align:center;"> 20 </td>
   <td style="text-align:center;"> 7.842194 </td>
   <td style="text-align:center;"> 9984 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 41 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 </td>
   <td style="text-align:center;"> 2015-08-01 15:10:00 </td>
   <td style="text-align:center;"> 2015-08-01 15:20:00 </td>
   <td style="text-align:center;"> 0.1666667 </td>
   <td style="text-align:center;"> 0.9166667 </td>
   <td style="text-align:center;"> 4 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 1001.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 1004.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 3.00 </td>
   <td style="text-align:center;"> 27 </td>
   <td style="text-align:center;"> 56 </td>
   <td style="text-align:center;"> 0.0000000 </td>
   <td style="text-align:center;"> 1.0000000 </td>
   <td style="text-align:center;"> 0.0210526 </td>
   <td style="text-align:center;"> 0.1443214 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0.0107527 </td>
   <td style="text-align:center;"> 0.1036952 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0.0102999 </td>
   <td style="text-align:center;"> 36 </td>
   <td style="text-align:center;"> 10.392305 </td>
   <td style="text-align:center;"> 9984 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 </td>
   <td style="text-align:center;"> 2015-08-01 04:30:00 </td>
   <td style="text-align:center;"> 2015-08-01 04:40:00 </td>
   <td style="text-align:center;"> 0.1666667 </td>
   <td style="text-align:center;"> 0.9166667 </td>
   <td style="text-align:center;"> 4 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1001.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 1004.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 3.00 </td>
   <td style="text-align:center;"> 30 </td>
   <td style="text-align:center;"> 76 </td>
   <td style="text-align:center;"> 0.3333333 </td>
   <td style="text-align:center;"> 0.6666667 </td>
   <td style="text-align:center;"> 0.0210526 </td>
   <td style="text-align:center;"> 0.1443214 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0.0107527 </td>
   <td style="text-align:center;"> 0.1036952 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0.0102999 </td>
   <td style="text-align:center;"> 20 </td>
   <td style="text-align:center;"> 9.451631 </td>
   <td style="text-align:center;"> 424 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 34 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-01 </td>
   <td style="text-align:center;"> 2015-08-01 10:00:00 </td>
   <td style="text-align:center;"> 2015-08-01 10:10:00 </td>
   <td style="text-align:center;"> 0.1666667 </td>
   <td style="text-align:center;"> 0.9166667 </td>
   <td style="text-align:center;"> 4 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1001.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 1004.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 3.00 </td>
   <td style="text-align:center;"> 43 </td>
   <td style="text-align:center;"> 113 </td>
   <td style="text-align:center;"> 0.3333333 </td>
   <td style="text-align:center;"> 0.6666667 </td>
   <td style="text-align:center;"> 0.0210526 </td>
   <td style="text-align:center;"> 0.1443214 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0.0107527 </td>
   <td style="text-align:center;"> 0.1036952 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0.0102999 </td>
   <td style="text-align:center;"> 13 </td>
   <td style="text-align:center;"> 4.509250 </td>
   <td style="text-align:center;"> 9984 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 39 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-02 </td>
   <td style="text-align:center;"> 2015-08-02 11:00:00 </td>
   <td style="text-align:center;"> 2015-08-02 11:10:00 </td>
   <td style="text-align:center;"> 0.1666667 </td>
   <td style="text-align:center;"> 1.4166667 </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1004.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 1001.188 </td>
   <td style="text-align:center;"> 0.5439056 </td>
   <td style="text-align:center;"> 3.75 </td>
   <td style="text-align:center;"> 19 </td>
   <td style="text-align:center;"> 39 </td>
   <td style="text-align:center;"> 0.3333333 </td>
   <td style="text-align:center;"> 0.6666667 </td>
   <td style="text-align:center;"> 0.0107527 </td>
   <td style="text-align:center;"> 0.1036952 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0.0319149 </td>
   <td style="text-align:center;"> 0.2296387 </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:center;"> 0.0211622 </td>
   <td style="text-align:center;"> 21 </td>
   <td style="text-align:center;"> 8.144528 </td>
   <td style="text-align:center;"> 9984 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 40 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:center;width: 5cm; font-weight: bold;"> 2015-08-02 </td>
   <td style="text-align:center;"> 2015-08-02 11:30:00 </td>
   <td style="text-align:center;"> 2015-08-02 11:50:00 </td>
   <td style="text-align:center;"> 0.3333333 </td>
   <td style="text-align:center;"> 1.4166667 </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1004.938 </td>
   <td style="text-align:center;"> 0.4425306 </td>
   <td style="text-align:center;"> 1001.188 </td>
   <td style="text-align:center;"> 0.5439056 </td>
   <td style="text-align:center;"> 3.75 </td>
   <td style="text-align:center;"> 23 </td>
   <td style="text-align:center;"> 109 </td>
   <td style="text-align:center;"> 0.2000000 </td>
   <td style="text-align:center;"> 0.8000000 </td>
   <td style="text-align:center;"> 0.0107527 </td>
   <td style="text-align:center;"> 0.1036952 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0.0319149 </td>
   <td style="text-align:center;"> 0.2296387 </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:center;"> 0.0211622 </td>
   <td style="text-align:center;"> 30 </td>
   <td style="text-align:center;"> 6.913754 </td>
   <td style="text-align:center;"> 9984 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 39 </td>
   <td style="text-align:center;"> NA </td>
   <td style="text-align:center;"> 0 </td>
  </tr>
</tbody>
</table></div>

  
These functions also calculate summary statistics for each event (e.g. flight bout). 

These include:

* **date** :  Date (without time)
* **start** : Start time and date of the event, `POSIXct` format 
* **end** :  Time and date that the event finished, `POSIXct` format 
* **duration** : How long it lasted (in hours)
* **total_daily_duration** : The total duration of all the events that occurred that day (in hours)
* **total_daily_event_number** : The total number of events which occurred that day
* **cum_pressure_change** : The cumulative change in atmospheric pressure during that event (in hectopascals)
* **cum_altitude_change** : The cumulative change in altitude during that event (in metres)
* **cum_altitude_up** : The cumulative number of metres that the bird went upwards during that event 
* **total_daily_P_change** : The cumulative change in atmospheric pressure for all the events for that date (in hectopascals)
* **P_dep_arr** : The difference between atmospheric pressure at the start of the event, and at the end (in hectopascals)
* **pressure_range** : The total range of the atmospheric pressure during that event (maximum minus miniimum - in hectopascals)
* **altitude_range** : The total altitude range during that event (maximum minus miniimum - in metres)
* **mean_night_P** : The mean pressure during the night before the event took place (in hectopascals)
* **sd_night_P** : The standard deviation of pressure the night before the event took place (in hectopascals)
* **mean_nextnight_P** : The mean pressure the night after the event took place (in hectopascals)
* **sd_nextnight_P** : The standard deviation of pressure the night after the event took place (in hectopascals)
* **night_P_diff** : The difference between the mean pressures of the night before and the night after the event took place (in hectopascals)
* **median_activity** : The median activity during that event
* **sum_activity** : The sum of the activity during that event
* **prop_resting** : The proportion of time during that event where activity = 0
* **prop_active** : The proportion of time during that event where activity > 0
* **mean_night_act** : The mean activity during the night before the event took place
* **sd_night_act** : The standard deviation of activity the night before the event took place
* **sum_night_act** : The summed activity during the night before the event took place
* **mean_nextnight_act** :The mean activity the night after the event took place
* **sd_nextnight_act** : The standard deviation of activity the night after the event took place
* **sum_nextnight_act** : The summed activity the night after the event took place
* **night_act_diff** : The difference between the mean activity of the night before and the night after the event took place 
* **median_pitch** :  The median pitch during that event
* **sd_pitch** : The standard deviation of pitch during that event
* **median_light** :   The median light recordings during that event
* **nightime** : Whether or not it was night during the majority of the event (1= night, 0 = day)
* **median_gX** : Median raw acceleration on the x axis during the event
* **median_gY** : Median raw acceleration on the y axis during the event
* **median_gZ** : Median raw acceleration on the z axis during the event
* **median_mX** : Median raw magnetic field on the x axis during the event
* **median_mY** : Median raw magnetic field on the y axis during the event
* **median_mZ** : Median raw magnetic field on the z axis
* **median_temp** : Median temperature during the event (in celsius)
* **sd_temp** : Standard deviation of temperature during the event (in celsius)
* **cum_temp_change** : Cumulative absolute difference in temperature during the event (in celsius)  
 
