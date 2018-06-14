---
layout: post
title: "Easy web maps with R and leaflet"
date: 2015-03-16 09:00:00
comments: true
categories: [R, maps, GIS, leaflet]
published: true
status: publish
---
 
I've been using R as a [Geographic Information System](https://en.wikipedia.org/wiki/Geographic_information_system) (GIS) for a long time, and I've relied heavily on the excellent [rgdal](http://cran.r-project.org/web/packages/rgdal/index.html), [maptools](http://cran.r-project.org/web/packages/maptools/index.html), and [raster](http://cran.r-project.org/web/packages/raster/index.html) packages to process spatial data.  One thing has always bugged me about this workflow, however. If I want to check or explore geographic data by clicking around on a map, I nearly always have to export it and open it up in another program like [ArcGIS](http://www.esri.com/software/arcgis) or [QGIS](http://www2.qgis.org/en/site/). That's why I was really excited to learn that the folks at [Rstudio](http://www.rstudio.com/) have come up with a cool solution: they've written R wrappers for the [leaflet](http://leafletjs.com/) javascript library which allows folks to quickly overlay vector data on an interactive web map.
 
In this post I'll describe how to build this map and share it on Rpubs or a self-hosted website. This assumes you have some familiarity with R and manipulating spatial data. For a great compendium of resources on GIS in R, see [here](http://spatial.ly/r/)
 
<!-- more -->
 
First, here's the finished map with a legend. This is the end product we are shooting for:
 

    ## quartz_off_screen 
    ##                 2

![plot of chunk unnamed-chunk-1](/images/figure/unnamed-chunk-1-1.png)
 
The map shows the number of accurately geo-tagged photos uploaded to the Flickr and iNaturalist websites from 2009 to 2013 in Mt. Rainier National Park, WA. The size and color of the dots is proportional to the number of photos appearing in 500m square bins overlayed on the park, and the park boundary is shown in white. We are using this data to monitor the timing of seasonal wildflower blooms, a project called [MeadoWatch](http://www.meadowatch.org)
 
##Installing the leaflet R package##
 
We will build this map up layer by layer, using data that is available [here](https://github.com/ibreckhe/ibreckhe.github.io/raw/source/source/data/MORA_flickr_data.zip), but first we need to set up our R workspace:
 

    require(rgdal)
    require(maptools)
    setwd('<your working directory>')
 
Next we need to install the leaflet R package. It's not up on CRAN yet, but you can install it from [GitHub](https://github.com)
 

    require(devtools)
    devtools::install_github("rstudio/leaflet")
##Making a basic map##
 
Leaflet overlays R data onto tiled web maps of various sources. To use the default tile set ([OpenStreetMap](http://www.openstreetmap.org/)), all you need to do is call the `leaflet()` function and add tiles to it using the `addTiles()` function. If you are using the newest version of Rstudio, then this map appears alongside your code in the viewer pane.
 

    leaflet() %>% addTiles()

![plot of chunk unnamed-chunk-4](/images/figure/unnamed-chunk-4-1.png)
 
The syntax might be a bit unfamiliar because it uses the pipe (`%>%`) operator. What it does is input the results of the left-hand expression as the first argument of the right-hand expression. The main advantage of using the pipe is that it allows related operations to be chained together without nesting functions using the confusing `f(g(x))` syntax. More on the pipe operator can be found [here](http://cran.r-project.org/web/packages/magrittr/vignettes/magrittr.html). 
 
##Adding custom tiles##
 
If your primary goal is to explore geographic data, then the default tile set might do just fine. If you want to share your map with others, however, it's nice to have some creative control over how the tiles look. Luckily, it's pretty straight-forward to change the background map. Many web mapping services use a standard URL scheme to organize access to the individual tile images, so we can change tile sets by specifying a URL template.
 
This template has a fixed portion, and then a few regions that change based on the geographic coordinates and the zoom level. For our map, we are going to use some custom tiles I designed with [MapBox](https://www.mapbox.com/). To access the tiles, we need to build the URL template that includes the path to the tiles and the access token:
 

    MBaccessToken <- "pk.eyJ1IjoiaWJyZWNraGUiLCJhIjoidVNHX1VpRSJ9.9fPQ1A3rdxyCAzPkeYSYEQ"
    MBurlTemplate <- "https://a.tiles.mapbox.com/v4/ibreckhe.map-z05003mi/{z}/{x}/{y}.png?access_token="
    MBTemplate <- paste(MBurlTemplate,MBaccessToken,sep="")
Now we can add these tiles to our new map. Because we will continue to add layers to this map, we store the output as an object `m`, and then zoom in to our study area, Mt. Rainier National Park:
 

    m <- leaflet() %>% addTiles(MBTemplate) %>% setView(lat=46.854039, lng=-121.760366, zoom = 10)
    m

![plot of chunk unnamed-chunk-6](/images/figure/unnamed-chunk-6-1.png)
 
##Preparing data for leaflet##
 
Next we want to overlay some of our own data on this map. You can download the data I'm using [here](https://github.com/ibreckhe/ibreckhe.github.io/raw/source/source/data/MORA_flickr_data.zip).
 
Before we add the data to the map, we need to re-project the data into the coordinate system used by leaflet and most web maps: WGS84 Geographic Coordinates. To do this we will read our data into R, assign a coordinate system, and then use the `spTransform` function from the package `rgdal` to do the heavy-lifting:
 

    ##Loads and formats data.
    photos <- read.csv("MORA_grid_500m_flickr.csv")
    photos <- photos[photos$PNTCNT>0,]
    coordinates(photos) <- ~XCTR + YCTR
    proj4string(photos) <- "+proj=utm +zone=10 +ellps=GRS80 +units=m +no_defs"
    photos_latlng <- spTransform(photos,CRSobj=CRS("+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs"))
     
    parkbound <- readShapePoly("MORA_boundary_UTM10.shp")
    proj4string(parkbound) <- "+proj=utm +zone=10 +ellps=GRS80 +units=m +no_defs"
    parkbound_latlng <- spTransform(parkbound,CRSobj=CRS("+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs"))
 
##Adding data to the map##
 
Now the layers are in a geographic coordinate system suitable for overlay onto a web map. We can add the park boundary as a polygon using the `addPolygons` function, specifying the fill and line colors. There are corresponding `add*`functions for all of the major types of vector geographic data.
 

    m <- m %>% addPolygons(data=parkbound_latlng,
                           color="white",fillColor="white",
                           opacity=0.7,fillOpacity=0.1)
    m

![plot of chunk unnamed-chunk-8](/images/figure/unnamed-chunk-8-1.png)
 
##Visualizing data with size and color##
 
One of the most powerful things about GIS programs is their ability to display geographic data with symbols that vary in size, color, or shape depending on the values of their attributes. Next, we want to overlay the point dataset on the map to identify the locations where the most photos are taken.
 
To do this, we are going to scale the size and color of the points according to the number of photos at that location. The first part of the code re-scales the variable PNTCNT to a log scale that lies in the interval 0-1.
 

    trans_fun <- function(x){
      x_log <- log(x+1)*100
      range_log <- log(photos_latlng@data$PNTCNT+1)*100
      return(((x_log - min(range_log)) / max(range_log)))
    }
    vals <- trans_fun(photos_latlng@data$PNTCNT)
Next we use `colorRamp()` to create a function that maps colors to values:
 

    ramp <- colorRamp(c("white","blue"))
    cols_rgb <- ramp(vals)
    cols <- rgb(cols_rgb,maxColorValue=256)
 
The vector `cols` now contains hex color values that map to the re-scaled values of PNTCNT. We don't need to be quite as elaborate with our calculations of size:

    sizes <- log(photos_latlng@data$PNTCNT+1)*2
Now we can add the points to our map:
 

    m  <- m %>% addCircleMarkers(lng=coordinates(photos_latlng)[,1],
                     lat=coordinates(photos_latlng)[,2],
                     radius=sizes,
                     fillColor=cols,fillOpacity=0.9,
                     weight=0.5)
    m

    ## Error in (function (url = NULL, file = "webshot.png", vwidth = 992, vheight = 744, : webshot.js returned failure value: 2
 
##Adding a legend##
 
Unfortunately, the leaflet package doesn't yet have good built-in capabilities for adding a legend, so we will have to use a work-around. 
In the bottom-right of each map is small transparent panel called the "Attribution" panel. This is important because lots of tile map providers require that you give them credit, and the attribution panel allows you to do that. We will take advantage of the fact that the addTiles() function allows us to insert arbitrary HTML into this field, and the result is rendered on the map. Since this only accepts plain HTML (no embedded CSS), we need to create the legend as a separate image, and then host it somewhere. In this case, I'm using the base function `legend()` to create an image of the legend, and then uploading it to this page hosted on GitHub.
 

    leg_breaks <- c(1,10,100)
    leg_size <- log(leg_breaks+1)
    leg_col <- rgb(ramp(trans_fun(leg_breaks)),maxColorValue=256)
     
    png(filename="/Users/ian/code/ibreckhe.github.io/source/images/web_map_legend.png",
        width=50,height=70,unit="px",bg=rgb(0,0,0,0))
    par(mar=c(1,1,0,0))
    plot(leg_breaks,type="n",axes=FALSE,xlab="",ylab="")
    legend("center",pch=21,col="black",pt.bg=leg_col,pt.cex=leg_size,
           legend=c("1","10","100"),bty="n",pt.lwd=1,y.intersp=1.2,xpd=TRUE)
    dev.off()

    ## quartz_off_screen 
    ##                 2
 
Finally we can add the HTML legend including the image we just created:
 

    ##HTML for the legend:
    leg_html <-   "<div class='my-legend'>
                  <div class='legend-title'>Flickr Photos <br/> (2009-2013)</div>
                  <div class='legend-scale'>
                  <img src='https://raw.githubusercontent.com/ibreckhe/ibreckhe.github.io/master/images/web_map_legend.png' alt='Legend' style='width:50px;height:70px'>
                  </div>
                  <div class='legend-source'>Map: <a href='http://ibreckhe.github.io'>Ian Breckheimer</a><br/>
                      Data: <a href='http://www.flickr.com'>Flickr</a></div>
                  </div>"
     
    m <- m %>% addTiles(MBTemplate,attribution=leg_html)
    m

    ## Error in (function (url = NULL, file = "webshot.png", vwidth = 992, vheight = 744, : webshot.js returned failure value: 2
 
##Adding interactive content##
 
Using the leaflet javascript library with tools like D3 allow for virtually unlimited customization of interactive maps. The R bindings for interactivity in the current leaflet package are pretty simple, but they give us the ability to make maps with pop-up markers that reveal more information. Here's a basic example:
 

    paradise <- c(46.785571, -121.736838)
    sunrise <- c(46.914467, -121.643628)
    spray <- c(46.920867, -121.834865)
    popups <- data.frame(rbind(paradise,sunrise,spray))
    popups$Label <- c("Paradise","Sunrise","Spray Park")
     
    colnames(popups) <- c("latitude","longitude","Label")
     
    ##Adds Popup points of interest.
    m <- m %>% addMarkers(lat=popups[,1],lng=popups[,2],popup=popups$Label,options=markerOptions(clickable=TRUE))
    m

    ## Error in (function (url = NULL, file = "webshot.png", vwidth = 992, vheight = 744, : webshot.js returned failure value: 2
We've now labeled the three of the most-photographed locations on our map! Click on the markers to see the pop-ups.
 
##Publishing the map##
 
Now that we have a serviceable map, we can share it with others. There are (at least) three ways to do that:
 
1. **Export the map widget as a standalone HTML file.** This file can be shared with anyone, and as long as they have a modern web browser and an internet connection, they should be able to see your map. You can do this in Rstudio by select *"Export to HTML..."* in the viewer pane.
 
2. **Push the widget to Rpubs.** If you are working in RStudio, then you can also upload the map to the Rpubs figure-sharing service by clicking the button in the top-right corner of the viewer pane or selecting *"Export>Publish to Rpubs..."*
 
2. **Host the widget on a website using Github Pages.** The html widget can be dropped into any webpage, and will often display just fine. I found that hosting these maps on my own site was a bit of a challenge, and I needed to make sure that the page headers included links to a few scripts provided by Rstudio.
