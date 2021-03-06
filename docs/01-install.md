# Installing PAMLr {#install}


## Prerequisites

To install this package from github, `devtools` must already be installed.


```r
install.packages("devtools")
```


## Installing


_PAMLr_ can then be installed from github using the following code:


```r
devtools::install_github("KiranLDA/PAMLr")
```


Note that if there are any problems with installing _PAMLr_, it is worth checking that the packages that _PAMLr_ relies on are correctly installed (using `install.packages()`). These include:

* changepoint 
* cluster 
* data.table
* depmixS4 
* dplyr
* dygraphs 
* EMbC 
* GeoLight 
* graphics 
* grDevices 
* htmltools 
* raster
* rgl 
* viridis
* xts
* zoo

Any errors such as: 

```
Error in loadNamespace(i, c(lib.loc, .libPaths()), versionCheck = vI[[i]]) : 
  there is no package called ‘rlang’
```
Can be dealt with through:


```r
install.packages("rlang")
```
