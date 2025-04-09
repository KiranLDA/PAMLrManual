# Example 3: flap-glide {#swift}



<div class="figure" style="text-align: center">
<img src="https://upload.wikimedia.org/wikipedia/commons/e/ec/Tachymarptis_melba_-Spain_-collage-8.jpg" alt="Photograph by Pau Artigas, (c) creative commons" width="100%" />
<p class="caption">(\#fig:appuuuuus)Photograph by Pau Artigas, (c) creative commons</p>
</div>




## Classifying migratory flight Alpine swifts: The dataset

Alpine swift (*Apus melba*) have been tracked on the their migrations from Switzerland to sub-Saharan Africa using SOI-GDL3pam loggers. 

* **Pressure** is recorded every 15 minutes
* **Light** is recorded every 2 minutes
* **Activity** is recorded every 5 minutes
* **Pitch** is recorded every 5 minutes
* **Temperature** is recorded every 15 minutes
* **Tri-axial acceleration** is recorded every 4 hours
* **Tri-axial magnetic field** is recorded every 4 hours



``` r
data("swift")

# make sure the cropping period is in the correct date format
start = as.POSIXct("2016-09-01","%Y-%m-%d", tz="UTC")
end = as.POSIXct("2017-04-10","%Y-%m-%d", tz="UTC")

# Crop the data
PAM_data= create_crop(swift,start,end)
```

## Visualise data

Sensor images are a good place to start when analysing data, as they can give a rapid overview of the dataset. Darker colors represent lower values, and lighter colors (in this case yellow) represent higher values. Sensor images for **activity** (also known as an actogram), **pitch**, **pressure** and **temperature** are a good place to start. The following code plots this for us:


``` r
par(mfrow= c(1,4), # number of panels
    oma=c(0,2,0,6), # outer margin around all panels
    mar =  c(4,1,4,1)) # inner margin around individual fivure

plot_sensorimage(PAM_data$acceleration$date, ploty=FALSE,
          PAM_data$acceleration$act, main = "Activity",
          col=c("black",viridis::cividis(90)), cex=1.2, cex.main = 2)

plot_sensorimage(PAM_data$acceleration$date, plotx=TRUE, ploty=FALSE, labely=FALSE,
          PAM_data$acceleration$pit,  main="Pitch",
          col=c("black",viridis::cividis(90)), cex=1.2, cex.main = 2)

plot_sensorimage(PAM_data$pressure$date, plotx=TRUE, ploty=FALSE, labely=FALSE,
          PAM_data$pressure$obs,  main="Pressure",
          col=c("black",viridis::cividis(90)), cex=1.2, cex.main = 2)

plot_sensorimage(PAM_data$temperature$date, labely=FALSE,
          PAM_data$temperature$obs,  main="Temperature",
          col=c("black",viridis::cividis(90)), cex=1.2, cex.main = 2)
```

<img src="10-swift_files/figure-html/sensooor3-1.png" width="100%" style="display: block; margin: auto;" />


## What should we look for?

Some pattern start to stick out.

* **Migration appears very short**
* The birds are **active all year round**
* Pressure is **lower during migration** indicating higher altitude flights, **particularly during night**
* Temperature is **lower during migration** also indicating higher altitudes


``` r
TOclassify = create_rolling_window(PAM_data,
                     resolution_out = 30 ,
                     window = 24*60,
                     interp = FALSE)
```

### Plot the classifiers

This helps think about which classifiers show differences for the behaviours we are trying to classify.


``` r
# choose variables of interest
varint = c("sd_pit",
           "sd_pressure",
           "median_temperature",
           "min_gZ",
           "max_act",
           "median_pressure",
           "median_mY",
           "sd_mZ")


#plot these variables of interest
par(mfrow=c(4,1), mar=c(4,4,0.5,0.5))
for (i in 1:length(varint)){
  plot(TOclassify$date, TOclassify[,varint[i]], 
       type="l", xlab="Date", ylab = varint[i])
}
```

<img src="10-swift_files/figure-html/unnamed-chunk-4-1.png" width="672" /><img src="10-swift_files/figure-html/unnamed-chunk-4-2.png" width="672" />

## Classify migration using a hidden markov model

One of the most difficult aspects of creating a classification is determining how many classes should be used. Here, we increase the number of classes until the behaviour we want to classify is correcly classified. Once this is done, we can extract this classification from the data.


``` r
# Select the columns to use as predictors in the model
predictor = TOclassify[, varint]

# Perform the classification
classification = classify_summary_statistics(predictor,
                             states = 7,
                             method = "hmm")
```

```
## converged at iteration 45 with logLik: -404572.4
```

```
## Warning in .local(object, ...): Argument 'type' not specified and will default
## to 'viterbi'. This default may change in future releases of depmixS4. Please
## see ?posterior for alternative options.
```

##Find which state is the migratory state

To do this, we find the state where pressure was the lowest, i.e. the bird was at the highest altitude, making the assumption that this is when the bird migrates.


``` r
#what is the minimum pressure for each class?
minP = unlist(lapply(1:length(unique(classification$cluster)),
       function(i) min(TOclassify$median_pressure[classification$cluster==i])))

# which class has the smallest minimum pressure
mig_state = which(minP == min(minP))

# create a new classification which is just migration, non-migration
mig_classification = ifelse(classification$cluster == mig_state,2,1)
```

## Plot the classification as a sensor image

Another way of looking at a classification is to use a sensor image of the results and to plot it side by side with the raw data to see if the same patterns are being picked out. We can also add (for instance sunset and sunrise events)


``` r
par(mfrow= c(1,3), # number of panels
    oma=c(0,2,0,6), # outer margin around all panels
    mar =  c(4,1,4,1)) # inner margin around individual fivure

col=c("royalblue3", "orange")

plot_sensorimage(PAM_data$acceleration$date, ploty=FALSE,
          PAM_data$acceleration$act, main = "Activity",
          col=c("black",viridis::cividis(90)), cex=1.2, cex.main = 2)

plot_sensorimage(PAM_data$pressure$date, plotx=TRUE, ploty=FALSE, labely=FALSE,
          PAM_data$pressure$obs,  main="Pressure",
          col=c("black",viridis::cividis(90)), cex=1.2, cex.main = 2)

plot_sensorimage(TOclassify$date, labely=FALSE,
          mig_classification, 
          main="Classification",
          col=col,
          cex=1.2, cex.main = 2)

# estimate sunrises and sunsets
twilights <- twilightCalc(PAM_data$light$date, PAM_data$light$obs,
                                    LightThreshold = 2, ask = FALSE)

# Add sunrises and sunsets
plot_sensorimage_twilight(twilights$tFirst,
       offset=0,
       col= ifelse(twilights$type == 1,
                   "grey","black"),
       pch=16, cex=0.5)
```

<img src="10-swift_files/figure-html/unnamed-chunk-7-1.png" width="100%" style="display: block; margin: auto;" />


