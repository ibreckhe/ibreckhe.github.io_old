---
layout: post
title: "Updated 2017 Snow Melt Forecasts for Mt. Rainier National Park"
date: 2017-06-21 09:00:00
comments: true
categories: [R, maps, GIS, MapBox]
published: true
status: publish
---
 
I've updated my seasonal snow melt forecasts for Mt. Rainier National Park to reflect the latest snow conditions in 2017. I'll update these maps approximately every two weeks through the early summer.
 
The link to the most up-to-date forecast map is [here](https://ibreckhe.github.io/snow_forecasts.html). If you zoom in on the map enough, you should see overlayed data for roads and trails!
 
<!-- more -->
 
As of June 21st, 2017, I'm making the following projections for snow melt timing for key parts of Mt. Rainier:
 
**Meadows around Reflection Lakes:** July 1st - July 10th
 
**Paradise Park above Jackson Visitor Center:** July 20th - August 1st
 
**Yakima Park around Sunrise Visitor Center:** July 10th - July 20th
 
**Lower Spray Park above Mowich Lake Camp:** July 15th - July 25th
 
**Meadows near Tipsoo Lake (Chinook Pass)** July 5th - July 15th
 
<a href="http://tinyplant.org/snow_forecasts"><img width=800 src="/images/forecast_6_7_17.png" alt="Snow Forecast Example" /></a>
 
## How is this forecast generated?
 
This forecast is a statistical estimate based on three key types of information:
 
*1. Historical SNOTEL Data.* The baseline data that informs this forecast is the relationship between the current size of the snowpack, and the date that snow eventually melted at the Paradise SNOTEL station during the 30 years of record. Snowpack melts later in years when more snow accumulates, so we can use that relationship to forecast the eventual date of melt at Paradise as long as we know how large the snowpack currently is.
 
*2. Microclimate Sensors.* To extrapolate this forecast spatially, we use the relationship between when snow melted at the Paradise SNOTEL station and when it melted at a network of ~400 snow duration sensors distributed throughout the park during the previous 6 years. These sensors allow us to estimate how much topography, canopy cover, and geographic position affect when snow melts.
 
*3. Expert knowledge.* The relationship between SNOTEL station melt dates and melt dates in other parts of the park varies from year to year, depending on the wind direction during winter storms and the average snow level during those storms. We use reports of snow from NPS crews, Washington DOT road closure reports, and information from other SNOTEL stations to select from the recent historical record the year where these patterns of snow accumulation most resemble the current year, and base our spatial forecasts on the spatial pattern of melt that most closely resembles the current year. We also adjust the forecast melt rates based on NOAA seasonal climate outlooks. If the outlook is for a warmer-than-average spring, then the forecast melt date is adjusted forward up to 10 days.
 
## How much uncertainty is there in the forecast?
 
That depends on when and where. The uncertainty is generally high early in the season, and decreases as the spring melt season progresses. Early-season forecasts have an average uncertainty (80% prediction interval) of plus-or-minus approximately 3 weeks, and that decreases to approximately plus-or-minus one week as snowpack at the Paradie SNOTEL station reaches 25% of it's peak seasonal value. Uncertainty is also high in parts of Mt. Rainier National Park where we have few measurements, including remote areas and high elevations (above 7000').
 
## What's up with that mid-elevation snow?
 
The early-season forecasts sometimes predict snow at mid-elevations (3000 - 4500ft) when there is no snowpack on the ground. This is because these elevations still are at risk of a late-spring snowstorm, and the forecast is for the last date of snow cover, not the melt date of persistent snowpack. At higher elevations at Mt. Rainier (above 4500ft) these two things are usually the same, but they differ at lower elevations where snow is not as persistant.
 
## Who put this forecast together?
 
The forecast was created by Ian Breckheimer and Janneke Hille Ris Lambers at the University of Washington, based on initial work done by Kevin Ford, now at the USDA Forest Service Pacific Northwest Research Station. Groundwork for the forecast was funded by NASA and in-kind support from the NW Climate Science Center and the National Park Service.
