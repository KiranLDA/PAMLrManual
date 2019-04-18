# Importing data {#import}

## Load PAMLr


```r
library(PAMLr)
```

## Use existing data

PAMLr has three integrated datasets for exploring the code. These include:

### Hoopoe (_Upupa epops_) 


```r
data("hoopoe")
```

<div class="figure" style="text-align: center">
<img src="https://upload.wikimedia.org/wikipedia/commons/2/2f/Common_Hoopoe_%28Upupa_epops%29_Photograph_by_Shantanu_Kuveskar.jpg" alt="Photograph by Shantanu Kuveskar, (c) creative commons" width="40%" />
<p class="caption">(\#fig:hoopoe)Photograph by Shantanu Kuveskar, (c) creative commons</p>
</div>

### European Bee-eater (_Merops apiaster_)


```r
data("bee_eater")
```

<div class="figure" style="text-align: center">
<img src="https://upload.wikimedia.org/wikipedia/commons/6/61/Gu%C3%AApier_d%27Europe_Merops_apiaster_-_European_Bee-eater_%28parc_national_de_l%27Ichkeul%29_02.jpg" alt="Photograph by El Golli Mohamed, (c) creative commons" width="40%" />
<p class="caption">(\#fig:beeeater)Photograph by El Golli Mohamed, (c) creative commons</p>
</div>


### Alpine Swift (_Apus melba_)


```r
data("swift")
```

<div class="figure" style="text-align: center">
<img src="https://upload.wikimedia.org/wikipedia/commons/c/c1/Alpine_Swift-2719.jpg" alt="Photograph by Rudraksha Chodankar, (c) creative commons" width="40%" />
<p class="caption">(\#fig:swift)Photograph by Rudraksha Chodankar, (c) creative commons</p>
</div>


## Import your own data

Importing data is easy with PAMLr. All data files should be found within the same directory which can be accessed through the `pathname` argument. Currently there are a list of supported file types, which include:

* ".pressure"
* ".glf"
* ".gle"
* ".acceleration"
* ".temperature"
* ".magnetic"

It's therefore possible to decide which of these to import. By default, all are imported.


```r
PAM_data = importPAM(pathname = "C:/Put/your/path/here",
                     measurements = c(".pressure", 
                                      ".glf",
                                      ".acceleration", 
                                      ".temperature", 
                                      ".magnetic")`
```

## Data details

Once the PAM data are imported, they are stored as a nested list - with each element in the list containing a dataframe of measurements per date. For more details on the format of the data, use:


```r
PAM_data = hoopoe
str(PAM_data)
```

```
## List of 6
##  $ id          : chr "16AJ"
##  $ pressure    :'data.frame':	37412 obs. of  2 variables:
##   ..$ date: POSIXct[1:37412], format: "2016-07-15 00:00:00" ...
##   ..$ obs : int [1:37412] 969 969 969 969 969 969 969 969 969 969 ...
##  $ light       :'data.frame':	112401 obs. of  2 variables:
##   ..$ date: POSIXct[1:112401], format: "2016-07-15 00:00:00" ...
##   ..$ obs : int [1:112401] 0 0 0 0 0 0 0 0 0 0 ...
##  $ acceleration:'data.frame':	111900 obs. of  3 variables:
##   ..$ date: POSIXct[1:111900], format: "2016-07-15 00:00:00" ...
##   ..$ pit : int [1:111900] 10 10 10 10 10 10 11 11 11 11 ...
##   ..$ act : int [1:111900] 0 0 0 0 0 0 2 0 0 0 ...
##  $ temperature :'data.frame':	36818 obs. of  2 variables:
##   ..$ date: POSIXct[1:36818], format: "2016-07-15 00:00:00" ...
##   ..$ obs : int [1:36818] 33 33 33 33 33 33 33 33 33 33 ...
##  $ magnetic    :'data.frame':	1559 obs. of  7 variables:
##   ..$ date: POSIXct[1:1559], format: "2016-07-15 00:00:00" ...
##   ..$ gX  : int [1:1559] 849 -487 211 505 725 -2048 454 -126 919 -886 ...
##   ..$ gY  : int [1:1559] -2035 -2182 -2601 -2581 -2507 -1847 -2582 -2650 -2437 -2574 ...
##   ..$ gZ  : int [1:1559] -1642 -1962 351 -20 118 -1626 41 -76 -152 -1327 ...
##   ..$ mX  : int [1:1559] -1600 -947 -2779 7 -1852 5844 1061 -118 -2196 2493 ...
##   ..$ mY  : int [1:1559] 15645 15610 15259 15549 15561 14545 16631 14548 15195 10924 ...
##   ..$ mZ  : int [1:1559] 5559 4627 6374 6147 5887 10881 5177 9000 6810 13793 ...
```

* __Pressure__ data are recorded in hectopascals, generally every 15/30 minutes

* __Light__ data are recorded in an arbitrary unit, generally every 2/5 minutes

* __Acceleration__ data are summarised into two variables: 
  + `act` which is short for "activity"", and is the sum of the difference in acceleration on the z-axis (i.e. "jiggle"). It is recorded every 5 minutes (summarised from 32 measurements - 10Hz)
  + `pit` which is short for "pitch", and is the relative position of the bird's body relative to the z axis. It is an average over 32 measurements and is summarised every 5 minutes.

* __Temperature__ data are recorded in degrees Celsius, generally every 15/30 minutes

* __Magnetic__ data are in fact the combined recordings from a tri-axial accelerometer and magnetometer.
  + `gX`, `gY` and `gZ` are snapshot tri-axial acceleration data, recorded every 4 hours.
  + `mX`, `mY` and `mZ` are snapshot tri-axial magentic field data, recorded every 4 hours.

## Temporal resolution of data

>__Note that different sensors and loggers often collect data at different time intervals__


Table: (\#tab:loggeres) Temporal resolution of difference multisensor loggers

Sensor                       Logger       Developper      Resolution       Life                     Reference
----------------   ----------------    -------------    ------------ ----------  ----------------------------
**Pressure**             PAM-logger              SOI       15/30 min     1 year   Dhanjal-Adams et al. (2018)
                    activity logger             CAMR              1h     1 year          Sjöberg et al.(2018)
                     OS barologgers              CLA           1 min    13 days         Shipley et al. (2017)
**Temperature**          PAM-logger              SOI       15/30 min     1 year         Liechti et al. (2018)
                    activity logger             CAMR             1 h     1 year          Sjöberg et al.(2018)
                     OS barologgers              CLA           1 min    13 days         Dreelin et al. (2018)
**Acceleration**
  activity               PAM-logger              SOI           5 min     1 year           Meier et al. (2018)
                    activity logger             CAMR              1h     1 year         Bäckman et al. (2017)
  pitch                  PAM-logger              SOI           5 min     1 year         Liechti et al. (2018)
  tri-axial              PAM-logger              SOI             4 h     1 year         Liechti et al. (2018)
**Magnetic**    
  tri-axial              PAM-logger              SOI             4 h     1 year         Liechti et al. (2018)



SOI = Swiss Ornithological Institute

CAMR = Centre for Animal Movement Research

CLA = Cornell Laboratory of Ornithology




## Cropping the data

Note that very often, a logger continues to record data before and after it is removed from a bird. For example, it might be transported in a rucksack or left in a laboratory until data are downloaded. It's therefore important to remove these incorrect datapoints. This can be done using `cutPAM`.

```r
# make sure the cropping period is in the correct date format
start = as.POSIXct("2016-07-01","%Y-%m-%d", tz="UTC")
end = as.POSIXct("2017-06-01","%Y-%m-%d", tz="UTC")

# Crop the data
PAM_data= cutPAM(hoopoe,start,end)
```



## Table references

Bäckman, J., Andersson, A., Alerstam, T., Pedersen, L., Sjöberg, S., Thorup, K., & Tøttrup, A. P. (2017). Activity and migratory flights of individual free-flying songbirds throughout the annual cycle: method and first case study. Journal of Avian Biology, 48(2), 309–319. doi:10.1111/jav.01068

Dhanjal-Adams, K. L., Bauer, S., Emmenegger, T., Hahn, S., Lisovski, S., & Liechti, F. (2018). Spatiotemporal Group Dynamics in a Long-Distance Migratory Bird. Current Biology, 28(17), 2824–2830.e3. doi:10.1016/j.cub.2018.06.054

Dreelin, R. A., Shipley, J. R., & Winkler, D. W. (2018). Flight Behavior of Individual Aerial Insectivores Revealed by Novel Altitudinal Dataloggers. Frontiers in Ecology and Evolution, 6, 182. doi:10.3389/fevo.2018.00182

Hedenström, A., Norevik, G., Warfvinge, K., Andersson, A., Bäckman, J., & Åkesson, S. (2016). Annual 10-Month Aerial Life Phase in the Common Swift Apus apus. Current Biology, 26(22), 3066–3070. doi:10.1016/J.CUB.2016.09.014

Liechti, F., Bauer, S., Dhanjal-Adams, K. L., Emmenegger, T., Zehtindjiev, P., & Hahn, S. (2018). Miniaturized multi-sensor loggers provide new insight into year-round flight behaviour of small trans-Sahara avian migrants. Movement Ecology, 6(1), 19. doi:10.1186/s40462-018-0137-1

Meier, C. M., Karaardıç, H., Aymí, R., Peev, S. G., Bächler, E., Weber, R., … Liechti, F. (2018). What makes Alpine swift ascend at twilight? Novel geolocators reveal year-round flight behaviour. Behavioral Ecology and Sociobiology, 72(3), 45. doi:10.1007/s00265-017-2438-6

Shipley, JR, Kapoor, J, Dreelin, RA, Winkler, DW. (2018) An open‐source sensor‐logger for recording vertical movement in free‐living organisms. Methods in  Ecol Evol.; 9: 465– 471. doi:10.1111/2041-210X.12893

Sjöberg, S., Pedersen, L., Malmiga, G., Alerstam, T., Hansson, B., Hasselquist, D., … Bäckman, J. (2018). Barometer logging reveals new dimensions of individual songbird migration. Journal of Avian Biology, 49(9), e01821. doi:10.1111/jav.01821
