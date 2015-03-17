---
layout: post
title: "Phenology and the Intersection of Two Curves"
date: 2014-07-19 09:00:00
comments: true
categories: [R, Phenology]
published: true
status: publish
---
 
My lab has been working on a project forecasting how the timing of flowering will vary over space and through time with climate change. To do this well, we need robust ways to quantify shifts in phenological timing. The approach we have taken so-far is to fit relatively simple functions to the observed data, but how do we quantify the amount of shift? Here's where numerical integration comes in. Here's an example with some simulated data. I've borrowed some of this code from [here](http://stats.stackexchange.com/questions/12209/percentage-of-overlapping-regions-of-two-normal-distributions).
 
<!-- more -->
 
![Rainier Meadow](/images/Rainier_Meadow.JPG)
 
First we define the two functions representing the two phenological curves from different species, times, or places. In this case, the curves are quasi-gaussian, with functional forms we can easily fit using logistic regression.
 

    f1 <- function(x,a1,b1,c1) { exp(a1 + b1 * x + c1 * x^2)/(1 + exp(a1 + b1 * x + c1 * x^2)) }
    f2 <- function(x,a2,b2,c2) { exp(a2 + b2 * log(x) + c2 * log(x)^2)/(1 + exp(a2 + b2 * log(x) + c2 * log(x)^2)) }
 
Next we want to convert the two functions into density functions (meaning that areas under the curves should sum to one). I'm doing this numerically to avoid a nasty integral. This works well unless the areas under the curve are really small.
 

    f1_dens <- function(x,a1,b1,c1) { 
      y <- f1(x,a1,b1,c1)
      yi <- integrate(f1, -Inf, +Inf, a1=a1, b1=b1, c1=c1)
      return(y/yi[[1]])
    }
     
    f2_dens <- function(x,a2,b2,c2) { 
      y <- f2(x,a2,b2,c2)
      yi <- integrate(f2, 1e-10, +Inf, a2=a2, b2=b2, c2=c2)
      return(y/yi[[1]])
    }
 
Now we can define a function that returns the minimum of the two curves wherever they overlap.
 

    min_f1f2_dens <- function(x, a1, b1, c1, a2, b2, c2) {
      f1 <- f1_dens(x,a1,b1,c1) 
      f2 <- f2_dens(x,a2,b2,c2)
      pmin(f1, f2)
    }
 
Now we can use some made-up parameters for each curve and plot them.
 

    a1 <- -65
    b1 <- 40
    c1 <- -6
     
    a2 <- -2
    b2 <- 7
    c2 <- -6
    xs <- seq(0.0001,5,by=0.01)
     
    y1s <- f1(xs,a1,b1,c1)
    y2s <- f2(xs,a2,b2,c2)
    y1d <- f1_dens(xs,a1,b1,c1)
    y2d <- f2_dens(xs,a2,b2,c2)
     
    yid <- min_f1f2_dens(xs, a1, b1, c1, a2, b2, c2)
    xpd <- c(xs, xs[1])
    ypd <- c(yid, yid[1])
     
    par(mfrow=c(1,1))
    plot(xs, y1s, type="n", ylim=c(0, max(y1d,y2d)),xlab="Time", ylab="Flowering Density")
    lines(xs, y1d, lty="solid")
    lines(xs, y2d, lty="dotted")

![plot of chunk unnamed-chunk-4](/images/figure/unnamed-chunk-4-1.png) 
 
The last step is to compute the area where the two curves intersect.
 

    ##Computes overlap
    over_dens <- integrate(min_f1f2_dens, 0, Inf, a1=a1, b1=b1, c1=c1, a2=a2, b2=b2, c2=c2)
 
Finally we can plot everything to make sure it makes sense.
 

    par(mfrow=c(1,1))
    plot(xs, y1s, type="n", ylim=c(0, max(y1d,y2d)),xlab="x", ylab="Density")
    polygon(xpd, ypd, col="gray")
    lines(xs, y1d, lty="solid")
    lines(xs, y2d, lty="dotted")
    title(main=paste("Overlap: ",round(over_dens[[1]],2)))

![plot of chunk unnamed-chunk-6](/images/figure/unnamed-chunk-6-1.png) 
 
There are some analytical ways to do this for polynomial and Gaussian functions, but this approach is agnostic about what the functional form of the phenology curves are, so long as they have a finite integral.
