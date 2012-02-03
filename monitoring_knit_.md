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
TIME=1:100
params = c(a,b,sigma)


REPS=10
poprep=function(init){x=numeric(100); x[1]=init; for(i in 1:99){x[i+1] = pop.bh(x[i], h=FISH,p=params)}; return(x)}
outs = sapply(rep(1,REPS), poprep)
plot(0,0, pch='', ylim=c(-.5,1.5),xlim=c(0,100), xlab='time', ylab='stock')
abline(h=(a-1)/b)
for(i in 1:dim(outs)[2]){lines(TIME,outs[,i],col=col.alpha('grey',0.9))}
lines(TIME,rowMeans(outs))


f.d <- as.data.frame(outs)
names(r.f) = paste("pop", 1:REPS, sep='')
f.d$time = TIME

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
for(i in 1:dim(outs)[2]){lines(TIME,outs[,i],col=col.alpha('grey',0.9))}
lines(TIME,rowMeans(outs))

r.d <- as.data.frame(outs)
names(r.d) = paste("pop", 1:REPS,sep='')
r.d$time <- TIME

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


![](https://github.com/ashander/sandbox/raw/master/power.png)


### Applied  ###

#### Status ####

Three "populations" sampled every 30 units


  
<!--begin.rcode est-recov-lik,echo=FALSE,cache=TRUE,warning=FALSE  
require(bbmle)
require(ggplot2)
SAMPLE=c(3,6,9)
r.d$mean <- 
r.m <- melt(r.d, id.vars=c('time'))
g <- ggplot()+geom_point(aes(time, value), color='darkgrey', data=r.m)
g <- g+geom_line(aes(time,mean(value)), data=r.m)
for(time in SAMPLE*10){
  sub.d <- list(tot=t(sample(r.d[time,1:10], size=9)))
  ml.t <- mle2(tot~dnorm(mean=mu, sd=sigma), data=sub.d, start=list(mu=mean(sub.d$tot), sigma=sd(sub.d$tot)))
  post.t <- sample.naive.posterior(ml.t)
  post.t$time = time
  g <- g + geom_boxplot(aes(time, mu), data=post.t)
         
}
g

end.rcode-->
  
**Likelihood**, treated like independent samples (no population model)

  
<!--begin.rcode est-recov-bayes,echo=FALSE,cache=TRUE,warning=FALSE  
g <- ggplot()+geom_point(aes(time, value), color='darkgrey', data=r.m)
g <- g+geom_line(aes(time,mean(value)), data=r.m)
prior <- NULL
for(time in SAMPLE*10){
  sub.d <- list(tot=t(sample(r.d[time,1:10], size=9)))
  ml.t <- mle2(tot~dnorm(mean=mu, sd=sigma), data=sub.d, start=list(mu=mean(sub.d$tot), sigma=sd(sub.d$tot)))
  post.t <- sample.naive.posterior(ml.t)
  if(is.null(prior)){
    prior <- c(coef(ml.t)['mu'], coef(ml.t)['sigma'])
  }
  if(!is.null(prior)){
    post.t$mu <- sample(post.t$mu, replace=TRUE, prob=dnorm(post.t$mu, mean=prior[1], sd=prior[2]))
    prior <- c(coef(ml.t)['mu'], coef(ml.t)['sigma'])
  }
  post.t$time = time
  g <- g + geom_boxplot(aes(time, mu), data=post.t)
         
}
g

end.rcode-->
  
**Bayes**, treated like "independent" samples


#### Trend ####

One "population" sampled every 10 units ...

<!--begin.rcode est-recov-lik-trend,echo=FALSE,cache=TRUE,warning=FALSE  
g <- ggplot()+geom_point(aes(time, value), color='darkgrey', data=r.m)
g <- g+geom_smooth(aes(time,value), data=r.m)
g <- g + geom_line(aes(time, pop1), data=r.d, color='darkgrey')
g <- g + geom_line(aes(time, pop2), data=r.d, color='darkgrey')
g <- g + geom_line(aes(time, pop3), data=r.d, color='darkgrey')
for(time in 1:9*10){
  sub.d <- list(tot=t(r.d[time,1:3]))
  ml.t <- mle2(tot~dnorm(mean=mu, sd=sigma), data=sub.d, start=list(mu=mean(sub.d$tot), sigma=sd(sub.d$tot)))
  post.t <- sample.naive.posterior(ml.t)
  post.t$time = time
  g <- g + geom_boxplot(aes(time, mu), data=post.t)
         
}
g

end.rcode-->
**Likelihood**


<!--begin.rcode est-recov-bayes-trend,echo=FALSE,cache=TRUE,warning=FALSE  
g <- ggplot()+geom_point(aes(time, value), color='darkgrey', data=r.m)
g <- g + geom_line(aes(time, pop1), data=r.d, color='darkgrey')
g <- g + geom_line(aes(time, pop2), data=r.d, color='darkgrey')
g <- g + geom_line(aes(time, pop3), data=r.d, color='darkgrey')
for(time in 1:9*10){
  sub.d <- list(tot=t(r.d[time,1:3]))
  ml.t <- mle2(tot~dnorm(mean=mu, sd=sigma), data=sub.d, start=list(mu=mean(sub.d$tot), sigma=sd(sub.d$tot)))
  post.t <- sample.naive.posterior(ml.t)
  if(is.null(prior)){
    prior <- c(coef(ml.t)['mu'], coef(ml.t)['sigma'])
  }
  if(!is.null(prior)){
    post.t$mu <- sample(post.t$mu, replace=TRUE, prob=dnorm(post.t$mu, mean=prior[1], sd=prior[2]))
    prior <- c(coef(ml.t)['mu'], coef(ml.t)['sigma'])
  }
  post.t$time = time
  g <- g + geom_boxplot(aes(time, mu), data=post.t)
         
}
g
end.rcode-->
**Bayes**


## Related issues ##

* power


* identifiability              
              
  

# Knitr #

Rewritten from sweave to the excellent [knitr](http://yihui.github.com/knitr/).

Need to use not only the options below 

<!--begin.rcode eval=FALSE
require(knitr)
opts_knit$set(out.format='gfm',base.url="https://github.com/ashander/sandbox/raw/master/")
knit(paste(getwd(),'monitoring_knit_.md',sep='/'))
end.rcode-->



