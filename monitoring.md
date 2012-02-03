
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

### Species (of concern) ###

## Issues in mointoring populations ##

* heterogeneity in space and time
* scales

>the design of a biological monitoring program requires careful attention to match sampling to the natural history of the population of interest.-- D Deutschman


## Issues: ## 

* Monitoring for status v trend with constant effort 
* Goals/objectives and the model
* Time scales

## Examples ## 

### status _v_ trend ###

![Status \\(\implies\\) Trend](https://github.com/ashander/sandbox/raw/master/dd-tradeoff.png)

>From Deutschman et al 2008 Tech Rpt.

### Bayesian example: random walk ###

### power ###




  
![plot of chunk plot1](https://github.com/ashander/sandbox/raw/master/plot1.png)







# Knitr #

Rewritten from sweave to the excellent [knitr](http://yihui.github.com/knitr/).

Need to use not only the options below 

```r
require(knitr)
opts_knit$set(out.format = "gfm", base.url = "https://github.com/ashander/sandbox/raw/master/")
knit(paste(getwd(), "monitoring_knit_.md", sep = "/"))
```



