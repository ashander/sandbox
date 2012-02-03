

Monitoring
======

**Jaime Ashander**


##What we monitor##


  
### Habitat###
* restored
* recovering
* extant?

### Biodiversity ###

### Resources ###

Fishery example





![plot of chunk resources](https://github.com/ashander/sandbox/raw/master/resources.png)```
## Error: subscript out of bounds
```



### Species (of concern) ###

Recovery example
  
![plot of chunk recovery](https://github.com/ashander/sandbox/raw/master/recovery.png)```
## Error: subscript out of bounds
```



## Issues in mointoring populations ##

* heterogeneity in space and time
* scales
* constrained resouorces

>the design of a biological monitoring program requires careful attention to match sampling to the natural history of the population of interest.-- D Deutschman


## Strategy ##


### Goals/objectives ###

### Model ###

### Scales (time and space) ###

Space:

* disconnect between model prediction and monitoring scale

Time:

>An example from marine reserves (White et al 2011 Frontiers)

* short: decrease in mortality
* medium-long: increased in larger ages, _uncertain_ due to influence of environment, connectivity, etc, and possibly cyclic due to resonance effects
* long: some interspecific interactions, 
* very long: physical changes, evolutionary change

## Tactics ##


### Monitoring for status v trend with constant effort ###

![](https://github.com/ashander/sandbox/raw/master/dd-tradeoff.png)

Tradeoff from status (left) to trend (right) with constant effort 
>From Deutschman et al 2008 Tech Rpt.

### Bayesian example: random walk ###

### power ###

  





# Knitr #

Rewritten from sweave to the excellent [knitr](http://yihui.github.com/knitr/).

Need to use not only the options below 

```r
require(knitr)
opts_knit$set(out.format = "gfm", base.url = "https://github.com/ashander/sandbox/raw/master/")
knit(paste(getwd(), "monitoring_knit_.md", sep = "/"))
```



