<!--roptions dev=png,width=5,height=5 -->

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

<!--begin.rcode model,echo=FALSE,cache=TRUE
require(rethinking) #for col.alpha
pop.bh <- function(x, h, p){
  x <- max(0, x - h)
  A <- p[1]
  B <- p[2]
  sig <- p[3]
  max(0, A * x/(1 + B * x) + rnorm(n=1, sd=sig))
}
end.rcode-->


<!--begin.rcode resources,echo=FALSE,cache=TRUE  
sigma = 0.1
a = 10
b = 9
FISH=0.3
params = c(a,b,sigma)


REPS=10
poprep=function(init){x=numeric(100); x[1]=init; for(i in 1:99){x[i+1] = pop.bh(x[i], h=FISH,p=params)}; return(x)}
outs = sapply(rep(1,REPS), poprep)
plot(0,0, pch='', ylim=c(-.5,1.5),xlim=c(0,100), xlab='time', ylab='stock')
abline(h=(a-1)/b)
for(i in 1:dim(outs)[2]){lines(1:100,outs[,i],col=col.alpha('grey',0.9))}

end.rcode-->

### Species (of concern) ###

Recovery example
  
<!--begin.rcode recovery,echo=FALSE,cache=TRUE  
a = 1 + 1e-9
b = 1e-9
FISH=0.0
params = c(a,b,sigma)
REPS=10

poprep=function(init){x=numeric(100); x[1]=init; for(i in 1:99){x[i+1] = pop.bh(x[i], h=FISH,p=params)}; return(x)}
outs = sapply(rep(0.1,REPS), poprep)
plot(0,0, pch='', ylim=c(-.5,1.5),xlim=c(0,100), xlab='time', ylab='population')
abline(h=(a-1)/b)
for(i in 1:dim(outs)[2]){lines(1:100,outs[,i],col=col.alpha('grey',0.9))}
end.rcode-->

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

<!--begin.rcode eval=FALSE
require(knitr)
opts_knit$set(out.format='gfm',base.url="https://github.com/ashander/sandbox/raw/master/")
knit(paste(getwd(),'monitoring_knit_.md',sep='/'))
end.rcode-->

