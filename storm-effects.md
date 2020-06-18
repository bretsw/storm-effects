---
title: "Storm Effects"
author: "K. Bret Staudt Willet"
date: "5/22/2020"
output: 
  html_document:
    keep_md: true
---

*Coursera Course: Reproducible Research, Peer Assessment 2*



## Synopsis

This report examines six decades (1950-2011) of storm event data from the U.S. [National Oceanic and Atmospheric Administration](https://www.noaa.gov/) (NOAA) Storm Database. The purpose of this study is to answer two questions related to the effects of storm events. Both questions consider storm events at a national scale in the United States. The first question pertains to population health effects of storms, as defined by injuries and fatalities. The total number of injuries and fatalities from tornadoes, heat, wind, flood, thunderstorms, and winter storms rate the highest need for concern. The second question pertains to economic consequences of storms, as defined by crop damage and property damage. The total dollar amount of crop damage and property damage from floods, wind, tornadoes, and hail rate the highest need for concern. Attention should be given to the three storm types on both lists: tornadoes, wind, and floods.

The code for this report can be viewed in full [on Github](https://github.com/bretsw/storm-effects/).

## Data Processing

The events in the dataset start in the year 1950 and end in November 2011. In the earlier years of the database there are generally fewer events recorded, most likely due to a lack of good records. More recent years should be considered more complete.

Because the file size, the dataset is saved in a compressed .bz2 file. However, this file type can still be read directly into R using the **dplyr** `read_csv()` function. Once the dataset has been loaded into R, I did some data processing, such as making the column names (i.e., variables) easier to work with using the `clean_names()` function from the **janitor** package. I also immediately removed odd storm event types, such as those denoted by "summary", "other", "none", and "?". 

The dollar amounts of property damage were listed in an odd way: a number value in one column (`propdmg`) and the order of magnitude, or exponential value, in a separate column (`propdmgexp`). I combined these two columns into one, whole number, value for property damage, `property_damage`.  Some of the `propdmgexp` values were unclear. So, for full disclosure, I interpreted the exponents to mean:

- **blank**, **NA**, and **0** mean no exponent (i.e., 10^0 = 1), or the identity
- **h** for hundred (100), **k** for kilo, or thousand (1,000), **m** for million (1,000,000), **b** for billion (1,000,000,000)
- **2-7** to mean 10^x, so 10^2, 10^3, 10^4, and so on



The next issue with the dataset is that the observed values of storm event types (`evtype`) contained typos, ambiguous entries, and similarly-themed-but-distinct entries.



Before cleaning, the dataset contained 902297 rows, or observations, of 976 different types of storm events. Closer examination showed that many of the these event types overlapped, sometimes due to typos, capitalization differences, or slight variations in phrasing (e.g., "wind gusts" vs. "wind advisory"). As a result, I used the `grepl()` function to identify different clusters of storm events, and I then consolidated these to reduce the number of categories of storm types.





This data cleaning process reduced the number of different storm types from 976 to 115.

## Results

This examination of storm event data sought to answer two questions related to the effects of storm events. Both questions consider storm events at a national scale in the United States. The first question pertains to population health effects of storms, as defined by injuries and fatalities. The second question pertains to economic consequences of storms, as defined by crop damage and property damage.

### Q1. Across the United States, which types of events are most harmful with respect to population health?

To create a plot of population health effects of storm events, I took these steps:

1. I grouped observations (i.e., rows) by storm event type.
1. I calculated the total injuries and fatalities, and mean injuries and fatalities, per event type.
1. I plotted fatalities (*y*-axis) by injuries (*x*-axis), on log10-scaled axes.



![](storm-effects_files/figure-html/unnamed-chunk-6-1.png)<!-- -->



**Figure 1.** Total fatalities per storm type by total injuries per storm type. Note that the *x*- and *y*-axes are shown on log10 scales to capture different orders of magnitude on a single plot. The total number of events by storm type is represented by the size of the points, and the severity of harm (mean number of injuries plus fatalities) per storm event is represented by the color gradient of the points.

As observed in Figure 1, the total number of injuries and fatalities from tornadoes, heat, wind, flood, thunderstorms, and winter storms rate the highest need for concern. 

### Q2. Across the United States, which types of events have the greatest economic consequences?

To create a plot of economic consequences of storm events, I took these steps:

1. Because property damage exponents (`propdmgexp` variable) of **+**and **-** were ambiguous, and rare (i.e., these accounted for just six rows out of 902,291), I removed these rows from the dataset.
1. I grouped observations (i.e., rows) by storm event type.
1. I calculated the total crop damage and property damage amount and mean crop damage and property damage amount, per event type.
1. I plotted property damage (*y*-axis) by crop damage (*x*-axis), on log10-scaled axes.



![](storm-effects_files/figure-html/unnamed-chunk-9-1.png)<!-- -->



**Figure 2.** Total property damage per storm type by total crop damage per storm type. Note that the *x*- and *y*-axes are shown on log10 scales to capture different orders of magnitude on a single plot. The total number of events by storm type is represented by the size of the points, and the severity of cost (mean number of property damage plus crop damage) per storm event is represented by the color gradient of the points.

As observed in Figure 2, the total dollar amount of crop damage and property damage from floods, wind, tornadoes, and hail rate the highest need for concern.
