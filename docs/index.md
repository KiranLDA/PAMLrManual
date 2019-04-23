--- 
title: "PAMLr Manual"
author: "Kiran Dhanjal-Adams"
date: "2019-04-23"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
link-citations: yes
description: "PAMLr Manual"
---

**A toolbox for analysing animal behaviour using Pressure, Acceleration, Temperature, Magnetic and Light data in R**




# Introduction

This is a user manual accompanying the R package **PAMLr**, which provides a set of functions for analysing behaviour from multisensor geolocator data - primarily for small migratory birds (however the methods are generalisable). The package is setup for SOI-GDL3**pam** loggers (developped by the [Swiss Ornithological Institute](www.vogelwarte.ch/en)) which measure atmospheric pressure (**P**), activity (**A**), magnetisim (**M**), temperautre and light. However, other multisensor geolocator data may be used, if it is formatted in the same format as SOI-GDL3pam data (see [Importing data](#import)).



<div class="figure" style="text-align: center">
<img src="images/PAM-logger.png" alt="SOI-GDL3pam logger, (c) Marcel Burkhardt,  Swiss Ornithological Institute" width="60%" />
<p class="caption">(\#fig:unnamed-chunk-1)SOI-GDL3pam logger, (c) Marcel Burkhardt,  Swiss Ornithological Institute</p>
</div>


## Index

* [Installing PAMLr](#install)
* [Importing data](#import)
* [Visualising data](#dataviz)
* [Common data patterns](#patterns)
* [Preparing data for analysis](#dataprep)
* [Analysis methods](#method)
* [Classification accuracy](#accuracy)
* [Classifying migratory flapping flight in Hoopoes](#flapping)
* [Classifying migratory flight in European bee-eaters](#soar)
* [Classifying migratory flight in Alpine Swifts](#swift)
* [A few last things to think about](#outook)

## Package citation

Dhanjal-Adams K.L., Willener A.S. T. & Liechti F. (201x) **PAMLr: a toolbox for analysing animal behaviour using Pressure, Acceleration, Temperature, Magnetic and Light data in R**, *Journal X*

## License

This project is licensed under the GNU General Public License version 3 - see the [LICENSE](https://github.com/KiranLDA/PAMLr/blob/master/LICENSE) file for details

