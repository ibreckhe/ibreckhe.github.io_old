---
layout: post
title: "Snow Melt Forecasts for Mt. Rainier National Park"
date: 2016-05-01 09:00:00
comments: true
categories: [R, maps, GIS, MapBox]
published: true
status: publish
---
 
I'm spending some time at [Mt. Rainier National Park](https://www.nps.gov/mora/) this spring as a "Scientist in Residence", a position made possible by support from the [Northwest Climate Science Center](https://www.nwclimatescience.org/). One of the projects that I'm working on is generating up-to-date forecasts of when Mt. Rainier's (massive) snowpack will finally melt this summer. The timing of snow melt is important for wildflower phenology (one of my research interests), but it's also important because it determines when some of the park's management activities can happen (i.e. trail maintenance, road openings), and potentially for visitors. This post serves as a brief FAQ to the forecasts, which are available live today.
 
The link to the most up-to-date forecast map is [here](https://ibreckhe.github.io/snow_forecasts.html).
 
<!-- more -->
 
![Snow Forecast](/images/forecast_4_28_16.png)
 
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
