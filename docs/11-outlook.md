# A few last things to think about {#outook}

## Cutting the data

Never forget that loggers can record data **even when they are not attached to an animal**. Often the logger is taken off, stored in a backpack, driven home or posted to a lab for download. Always make sure the analysis starts and stops when the logger was on the bird. The function `create_crop` is specifically setup for getting rid of these unwanted periods.

## Clock drift

As the battery runs out throughout the year, the clock on a logger can get gradually become slower and slower. There are a number of methods for correcting for this. The bird/animal will always be caught at a known location. It's best therefore to find the sunset and sunrise times for the known location, and to see by how many minutes the sunrise and sunset estimated from the light sensor differ from the true sunrise and sunset. It's then possible to linearly interpolate the timeseries by the known number of minutes. This can be implemented in using the function `calculate_clockdrift`.

## Using biologically meaningful patterns

The analysis of multisensor geolocator data can be approached from many angles. It's therefore important to use measures which are translatable into behaviour and to think carefully about the ecology of a species. Here are a few important points to keep in mind:


   * **Pressure can change as a result of weather, not just flight**. Think about the data resolution and use a weather station to investigate background pressure changes over the same time periods as the data are collected.
   * **Feathers and fur can tamper with sensor readings**. For instance feathers can cover light sensors or insulate the device from correclty recording temperature - be aware of these. They may even come to your advange, fo instance, a change in darkness could be used to understand moult.
   * **Do not use activity as a proxy for foraging success**. A bird could for instance be very active because it has to search a lot for food (low success rate). However, **resting** can definitely be used to quantify a lack of foraging.
   * **Activity** can result from many things - hopping or flapping. Think about it carefully when using it. For instance, high activity and high pressure change are likely to mean flying, but high acitivity and no pressure change are less clear. Pressure is only recorded every half hour, and activity every 5 minutes - so the bird could have flown in between the two pressure recordings but we would have no way of telling.
   
>**Recommendation: be aware of the limitations of the data**

## Data resolution and interpolation

The temporal resolution of the data **vary between sensors**. It's important to think about what resolution the analysis should be carried out at. For instance, with tri-axial measurements taken every 4 hours, it's impossible to perform dead reckoning, or to look at an individual flight event.

>**If there are missing data, be aware that some methods will not work for classifying behaviour**

If data are infrequent, refrain from using them for classification. If they are absolutely necessary, it's possible to interpolate linearly between two values. However, a better alternative would be to use a rolling window to derive statistics . See [data preparation](#dataprep) for more information.

## Using supervised machine learning

Here, the examples are geared towards migratory birds with tags recording less frequently over long periods. However, with an increase in data resolution over a shorter period, and on-ground observations, it's also possible to develop supervised learning techniques, look at dead reckoning, turning point classification, and much more, not just for birds. There are many possibilities.
