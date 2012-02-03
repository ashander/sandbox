<!--roptions dev=png,width=5,height=5 -->

Monitoring
======

Jaime Ashander
-----

## What we monitor## 

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
* Bayesian example: random walk 
* power?




  
<!--begin.rcode,echo=FALSE
sigma = 2

pop= function(x, sig){ x + rnorm(n=1,sd=sig)}

poprep=function(init){x=numeric(100); x[1]=init; for(i in 1:99){x[i+1] = pop(x[i], sigma)}; return(x)}

sapply(rep(10,100), poprep)

outs = sapply(rep(10,100), poprep)

plot(0,0, pch='', ylim=c(-20,40),xlim=c(0,100))

require(rethinking)
for(i in 1:100){ lines(1:100,outs[,i],col=col.alpha('grey',0.3))}

end.rcode-->


<!--begin.rcode,echo=FALSE
require(ggplot2)


end.rcode-->


# Knitr #

Rewritten from sweave to the excellent [knitr](http://yihui.github.com/knitr/).

Need to use not only the options below 

<!--begin.rcode eval=FALSE
require(knitr)
opts_knit$set(out.format='gfm',base.url="https://github.com/ashander/scifundstats/raw/master/")
knit(paste(getwd(),'monitoring_knit_.md',sep='/'))
end.rcode-->

