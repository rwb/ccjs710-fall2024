# CCJS 710: Advanced Statistics Methods - Limited Dependent Variables (Fall 2024)

* Instructor: Robert Brame (2139 LeFrak Hall; rbrame@umd.edu).
* Meetings -- This course is scheduled to meet on Thursdays from 4:00-6:45 in LeFrak 2165E.
* I plan to hold office hours on Wednesdays from 10-12:00 and by appointment.
* Course-related policies -- In all matters, I will follow University guidance as outlined [here](https://gradschool.umd.edu/faculty-and-staff/course-related-policies).
* Accessibility accommodations: If you think you might need one or more academic accommodations, please contact the Accessibility and Disability Service Office ([link](https://ads.umd.edu)) for guidance and assistance.
* Textbooks: Gary King (1989). *Unifying Political Methodology: The Likelihood Theory of Statistical Inference*. New York: Cambridge University Press; Larry Wasserman (2004). *All of Statistics: A Concise Course in Statistical Inference*. New York: Springer.
* Class Notes: each week, the class notes will be posted on this webpage.
* Statistical software: I will be using [R](https://www.r-project.org) for statistical computing and will provide a number of examples using R code. A primer on using R (written by Larry Wasserman) is available at this [website](https://www.stat.cmu.edu/~larry/all-of-statistics/=R/Rintro.pdf).
* All numeric grades assigned in this class  (including the final numeric grade in the class at the end of the semester) will be assigned on a 100-point scale and will be rounded off to the nearest 1 point (i.e,. a 78.7 would be rounded to a 79 and a 78.3 would be rounded down to a 78). Based on your numeric grade at the end of the semester, I will assign letter grades based on the following cutoffs:

<table>
<tr>
    <td>97 and higher: A+</td>
    <td>93-96: A</td>
    <td>90-92: A-</td>
</tr>
<tr>
    <td>87-89: B+</td>
    <td>83-86: B</td>
    <td>80-82: B-</td>
</tr>
    <td>77-79: C+</td>
    <td>73-76: C</td>
    <td>70-72: C-</td>
</tr>
<tr>
    <td>67-69: D+</td>
    <td>63-66: D</td>
    <td>60-62: D-</td>
</tr>
    <td> </td>
    <td> </td>
    <td>59 and lower: F</td>
</tr>
</table>

* Numeric grades: Your grade will be based on your performance on 7 assignments. You will have 1 week to work on each assignment and each assignment will count 1/7 weight toward your final grade. Each assignment will be graded on a 100-point scale.

### Course Outline

* 8/29: statistical inference review
* 9/5: statistical inference review
* Assignment #1 distributed on 9/5; due on 9/12.
* 9/12: proportions (K2.3,W2.3)
* 9/19: contingency tables (W15.1-15.2,K6.1-6.6)
* 9/26: measures of association (W15.5)
* Assignment #2 distributed on 9/26; due on 10/3.
* 10/3: logistic regression (W13.7,K5.1-5.2)
* 10/10: linear probability and probit models (K5.3)
* Assignment #3 distributed on 10/10; due on 10/17
* 10/17: bounded event count models (K5.5,W2.3; article [link](https://link.springer.com/article/10.1007/s10940-017-9346-9)).
* 10/24: unbounded event count models (W2.3,K5.7-5.8) 
* Assignment #4 distributed on 10/24; due on 10/31
* 10/31: multinomial logit models (W14.4; Agresti seat belt example)
* 11/7: ordinal logit/probit models (K5.4; Agresti seat belt example)
* Assignment #5 distributed on 11/7; due on 11/14
* 11/14: nonparametric survival time analysis (K9.1; article [link](https://link.springer.com/article/10.1007/BF01083132))
* 11/21: parametric survival models (W23.3)
* Assignment #6 distributed on 11/21; due on 12/5
* 11/28: Thanksgiving recess
* 12/5: partial identification with proportions (section 3.1 of article [link](https://www.jstor.org/stable/271005)).
* Assignment #7 distributed on 12/5; due during final exam week.

### Lesson 1 - Thursday 8/29/24

* We begin with a review/overview of statistical inference.
* Key terminology: estimator, estimate, bias, efficiency, mean squared error, random sampling, standard error, sample size, sampling distribution, confidence interval.

#### 1. Mean and Median for Normal Population Data

* Here is some R code:

```R
y <- rnorm(n=1000000,mean=100,sd=10)
mean(y)
sd(y)
hist(y)
```

```Rout
> y <- rnorm(n=1000000,mean=100,sd=10)
> mean(y)
[1] 100.0005
> sd(y)
[1] 10.00185
> hist(y)
>
```

* Next, let's draw repeated samples of size N = 300 from this population.
* R Code:

```
# Calculate population mean

popmean <- mean(y)

# draw a single sample of size n=[sampsize] 
# from the population with replacement

sampsize <- 300
yss <- sample(y,size=sampsize,replace=T)

# calculate the sample mean 

mean(yss)

# calculate the sample median 

median(yss)

# now, let's repeat this process [nsamples] times

nsamples <- 100000

ys_mean <- vector()
se_mean <- vector()
ys_median <- vector()

for(i in 1:nsamples)
  {
   ys <- sample(y,size=sampsize,replace=T)
   ys_mean[i] <- mean(ys)
   se_mean[i] <- sd(ys)/sqrt(sampsize)
   ys_median[i] <- median(ys)
   }

# calculate the mean squared error (mse)

ys_mean_mse <- sum((ys_mean-popmean)^2)/nsamples
ys_median_mse <- sum((ys_median-popmean)^2)/nsamples

# look at the results

c(mean(ys_mean),sd(ys_mean),ys_mean_mse)
c(mean(ys_median),sd(ys_median),ys_median_mse)
mean(se_mean)
median(se_mean)
```

Output:

```Rout
> # Calculate population mean
> 
> popmean <- mean(y)
> 
> # draw a single sample of size n=[sampsize] 
> # from the population with replacement
> 
> sampsize <- 300
> yss <- sample(y,size=sampsize,replace=T)
> 
> # calculate the sample mean 
> 
> mean(yss)
[1] 99.6635
> 
> # calculate the sample median 
> 
> median(yss)
[1] 99.30056
> 
> # now, let's repeat this process [nsamples] times
> 
> nsamples <- 100000
> 
> ys_mean <- vector()
> se_mean <- vector()
> ys_median <- vector()
> 
> for(i in 1:nsamples)
+   {
+    ys <- sample(y,size=sampsize,replace=T)
+    ys_mean[i] <- mean(ys)
+    se_mean[i] <- sd(ys)/sqrt(sampsize)
+    ys_median[i] <- median(ys)
+    }
> 
> # calculate the mean squared error (mse)
> 
> ys_mean_mse <- sum((ys_mean-popmean)^2)/nsamples
> ys_median_mse <- sum((ys_median-popmean)^2)/nsamples
> 
> # look at the results
> 
> c(mean(ys_mean),sd(ys_mean),ys_mean_mse)
[1] 100.0012125   0.5777509   0.3337933
> c(mean(ys_median),sd(ys_median),ys_median_mse)
[1] 100.0022306   0.7205418   0.5191784
> mean(se_mean)
[1] 0.5770371
> median(se_mean)
[1] 0.5768027
>
```

* What can we conclude based on this evidence?

#### 2. Sampling Variability of a Proportion

This case illustrates the greater sampling variability we get from drawing samples of size 50 in comparison to drawing samples of size 500. To create this simulation, we imagine a large population of prison releasees who have a true 3-year rearrest recidivism rate of 67.5%. The question is this: how close do we typically get to this estimate when we draw samples of size 50 and 500 from this population?

This case illustrates the greater sampling variability we get from drawing samples of size 50 in comparison to drawing samples of size 500. To create this simulation, we imagine a large population of prison releasees who have a true 3-year rearrest recidivism rate of 67.5%. The question is this: how close do we tycally get to this estimate when we draw samples of size 50 and 500 from this population?

```R
set.seed(1203842)

# set parameters for simulation

pi <- 0.675
y <- rbinom(n=10000000,size=1,p=pi) 

# population mean

mean(y)
```

and here is our output window:

```Rout
> set.seed(1203842)
> 
> # set parameters for simulation
> 
> pi <- 0.675
> y <- rbinom(n=10000000,size=1,p=pi) 
> 
> # population mean
> 
> mean(y)
[1] 0.6749167
>
```

Now, let's simulate drawing 10 samples of size 50 and 500 from this population:

```R
# set up repeated sampling loops

pvec <- vector() 
qvec <- vector()

for(i in 1:10) {
  s <- sample(1:length(y),size=50,replace=T) 
  ys <- y[s]
  pvec[i] <- mean(ys)
  }
  
pvec

for(i in 1:10) {
  s <- sample(1:length(y),size=500,replace=T) 
  ys <- y[s]
  qvec[i] <- mean(ys)
  }
  
 qvec
  ```

Here is our output from this simulation exercise:

```Rout
> # set parameters for simulation
> 
> pi <- 0.675
> y <- rbinom(n=10000000,size=1,p=pi) 
> 
> # population mean
> 
> mean(y)
[1] 0.6749167
> 
> # set up repeated sampling loops
> 
> pvec <- vector() 
> qvec <- vector()
> 
> for(i in 1:10) {
+   s <- sample(1:length(y),size=50,replace=T) 
+   ys <- y[s]
+   pvec[i] <- mean(ys)
+   }
>   
> pvec
 [1] 0.58 0.78 0.70 0.66 0.70 0.66 0.70 0.70 0.60 0.68
> 
> for(i in 1:10) {
+   s <- sample(1:length(y),size=500,replace=T) 
+   ys <- y[s]
+   qvec[i] <- mean(ys)
+   }
>   
>  qvec
 [1] 0.674 0.688 0.668 0.684 0.686 0.664 0.688 0.650 0.674 0.680
> 
```

* Here is a chart summarizing the results:
 
<p align="center">
<img src="/gfiles/rs3.png" width="800px">
</p>

#### 3. Confidence Intervals

Let's suppose there is a large population (1 million people) of prison releasees. Further suppose that 750,000 of these releasees get rearrested within 5 years of release -- implying a 75% recidivism rate. Here is the R code to enter the data:

```R
r <- c(rep(0,250000),rep(1,750000))
mean(r)
```

and the output is:

```Rout
> r <- c(rep(0,250000),rep(1,750000))
> mean(r)
[1] 0.75
> 
```

Next, we draw a sample of 1000 people from the original population and we calculate the recidivism rate and the standard error of the recidivism rate for the sample data:

```R
sampsize <- 1000

# note: rss means r for a single sample

rss <- sample(r,size=sampsize,replace=T)
mean(rss)
sqrt(mean(rss)*(1-mean(rss))/sampsize)
```

and the output is:

```Rout
> sampsize <- 1000
>
> # note: rss means r for a single sample
>
> rss <- sample(r,size=sampsize,replace=T)
> mean(rss)
[1] 0.75
> sqrt(mean(rss)*(1-mean(rss))/sampsize)
[1] 0.01369306
> 
```

Let's see how this compares to the actual sampling distribution:

```R
nsamples <- 100000

rs_est <- vector()
rs_std <- vector()

for(i in 1:nsamples)
  {
   rs <- sample(r,size=sampsize,replace=T)
   rs_est[i] <- mean(rs)
   rs_std[i] <- sqrt(mean(rs)*(1-mean(rs))/sampsize)
   }

# look at the results

mean(rs_est)
sd(rs_est)
mean(rs_std)
```

and the output is:

```Rout
> nsamples <- 100000
> 
> rs_est <- vector()
> rs_std <- vector()
> 
> for(i in 1:nsamples)
+   {
+    rs <- sample(r,size=sampsize,replace=T)
+    rs_est[i] <- mean(rs)
+    rs_std[i] <- sqrt(mean(rs)*(1-mean(rs))/sampsize)
+    }
> 
> # look at the results
> 
> mean(rs_est)
[1] 0.750027
> sd(rs_est)
[1] 0.01377905
> mean(rs_std)
[1] 0.01368331
> 
```

Let's now introduce the idea of a confidence interval. For each of our samples, let's calculate a 95% confidence interval around our point estimate. Then, let's see how often our confidence interval actually traps the true population recidivism rate of 0.75.

```R
nsamples <- 1000000

rs_est <- vector()
rs_std <- vector()
rs_lcl <- vector()
rs_ucl <- vector()

mult <- qnorm(p=0.975)
mult

for(i in 1:nsamples)
  {
   rs <- sample(r,size=sampsize,replace=T)
   rs_est[i] <- mean(rs)
   rs_std[i] <- sqrt(mean(rs)*(1-mean(rs))/sampsize)
   rs_lcl[i] <- rs_est[i]-mult*rs_std[i]
   rs_ucl[i] <- rs_est[i]+mult*rs_std[i]
   }

mean(rs_est)
sd(rs_est)
mean(rs_std)
flag <- rep(NA,nsamples)
flag[rs_lcl<=0.75 & rs_ucl>=0.75] <- 1
flag[rs_lcl>0.75 | rs_ucl<0.75] <- 0
table(flag)
```

and the output is:

```Rout
> nsamples <- 1000000
> 
> rs_est <- vector()
> rs_std <- vector()
> rs_lcl <- vector()
> rs_ucl <- vector()
> 
> mult.lower <- qnorm(p=0.025)
> mult.lower
[1] -1.959964
> 
> mult.upper <- qnorm(p=0.975)
> mult.upper
[1] 1.959964
> 
> for(i in 1:nsamples)
+   {
+    rs <- sample(r,size=sampsize,replace=T)
+    rs_est[i] <- mean(rs)
+    rs_std[i] <- sqrt(mean(rs)*(1-mean(rs))/sampsize)
+    rs_lcl[i] <- rs_est[i]+mult.lower*rs_std[i]
+    rs_ucl[i] <- rs_est[i]+mult.upper*rs_std[i]
+    }
> 
> mean(rs_est)
[1] 0.749979
> sd(rs_est)
[1] 0.01369093
> mean(rs_std)
[1] 0.01368431
> flag <- rep(NA,nsamples)
> flag[rs_lcl<=0.75 & rs_ucl>=0.75] <- 1
> flag[rs_lcl>0.75 | rs_ucl<0.75] <- 0
> table(flag)

     0      1 
 53333 946667
```

### Lesson 2 - Thursday 9/5/24

* Review of statistical inference (continued).
* Key terminology: least squares, maximum likelihood, linear regression, confidence interval of expected value

#### 4. Linear Regression Overview

* We begin by generating a dataset with known population parameter values.

```R
# set random number seed

set.seed(777)

# generate a single data set

nc <- 1000
x <- rbinom(n=nc,size=1,p=0.5)
e <- rnorm(n=nc,mean=0,sd=7)
y <- 50+x+e

# look at first few observations in the data set

d <- data.frame(x,y)
head(d,n=10)
```

* Output:

```Rout
> # set random number seed
> 
> set.seed(777)
> 
> # generate a single data set
> 
> nc <- 1000
> x <- rbinom(n=nc,size=1,p=0.5)
> e <- rnorm(n=nc,mean=0,sd=7)
> y <- 50+x+e
> 
> # look at first few observations in the data set
> 
> d <- data.frame(x,y)
> head(d,n=10)
   x        y
1  1 52.29635
2  0 55.61395
3  0 50.35588
4  1 55.67656
5  1 58.88501
6  0 55.30937
7  0 42.95104
8  0 43.72929
9  1 44.36395
10 0 51.05988
>
```

* Graphical overview of the data:

```R
par(mfrow=c(1,3))
hist(y)
hist(y[x==0])
hist(y[x==1])
```

* which generates the following set of histograms:

<p align="center">
<img src="/gfiles/h1.png" width="600px">
</p>

* and here is a boxplot:

```R
boxplot(y~x)
```

which give us this chart:

<p align="center">
<img src="/gfiles/bp1.png" width="600px">
</p>

* Next, we estimate a linear regression model (regressing y on x). We will also extract some terms from the model object, M.

```R
# estimate linear regression model

M <- lm(y~1+x)
summary(M)

# now we extract terms from this model
# define the linear model as y=a+bx+e

ahat <- coef(M)[1]
ahat
bhat <- coef(M)[2]
bhat
sigmahat <- sigma(M)
sigmahat
```

* Output:

```Rout
> # estimate linear regression model
> 
> M <- lm(y~1+x)
> summary(M)

Call:
lm(formula = y ~ 1 + x)

Residuals:
     Min       1Q   Median       3Q      Max 
-19.9040  -4.4970  -0.0656   4.5976  24.7341 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  50.1949     0.2964 169.348   <2e-16 ***
x             0.9789     0.4287   2.283   0.0226 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 6.772 on 998 degrees of freedom
Multiple R-squared:  0.005197,	Adjusted R-squared:  0.004201 
F-statistic: 5.214 on 1 and 998 DF,  p-value: 0.02261

> 
> # now we extract terms from this model
> # define the linear model as y=a+bx+e
> 
> ahat <- coef(M)[1]
> ahat
(Intercept) 
   50.19493 
> bhat <- coef(M)[2]
> bhat
        x 
0.9789385 
> sigmahat <- sigma(M)
> sigmahat
[1] 6.771959
>
```

* Next, we need to collect some terms for further calculations:

```R
# calculate sample means of x and y

xbar <- mean(x)
xbar
ybar <- mean(y)
ybar

# we can also extract the standard errors
# of the intercept and slope terms

vcov(M)

se.ahat <- sqrt(vcov(M)[1,1])
se.ahat

se.bhat <- sqrt(vcov(M)[2,2])
se.bhat
```

* Here is the output:

```Rout
> # calculate sample means of x and y
> 
> xbar <- mean(x)
> xbar
[1] 0.478
> ybar <- mean(y)
> ybar
[1] 50.66286
> 
> # we can also extract the standard errors
> # of the intercept and slope terms
> 
> vcov(M)
            (Intercept)          x
(Intercept)   0.0878533 -0.0878533
x            -0.0878533  0.1837935
> 
> se.ahat <- sqrt(vcov(M)[1,1])
> se.ahat
[1] 0.2964006
> 
> se.bhat <- sqrt(vcov(M)[2,2])
> se.bhat
[1] 0.4287115
>
```

* We can use standard textbook formulas to get the same results:

```R
# first, estimate the slope term

slope.pt1 <- sum((x-xbar)*(y-ybar))
slope.pt2 <- sum((x-xbar)^2)
slope.est <- slope.pt1/slope.pt2
slope.est

# second, estimate the intercept term

int.est <- ybar-slope.est*xbar
int.est

# third, we estimate the regression 
# root mean square error term (sigma)

n <- length(x)
yp <- int.est+slope.est*x
esq <- (yp-y)^2
sigma.est <- sqrt((1/(n-2))*sum(esq))
sigma.est

# now, we calculate the standard error of the slope

se.slope <- sigma.est/(sd(x)*sqrt(n))
se.slope

# and the standard error of the intercept

se.int <- se.slope*sqrt(sum(x*x)/n)
se.int
```

* Output:

```Rout
> # first, estimate the slope term
> 
> slope.pt1 <- sum((x-xbar)*(y-ybar))
> slope.pt2 <- sum((x-xbar)^2)
> slope.est <- slope.pt1/slope.pt2
> slope.est
[1] 0.9789385
> 
> # second, estimate the intercept term
> 
> int.est <- ybar-slope.est*xbar
> int.est
[1] 50.19493
> 
> # third, we estimate the regression 
> # root mean square error term (sigma)
> 
> n <- length(x)
> yp <- int.est+slope.est*x
> esq <- (yp-y)^2
> sigma.est <- sqrt((1/(n-2))*sum(esq))
> sigma.est
[1] 6.771959
> 
> # now, we calculate the standard error of the slope
> 
> se.slope <- sigma.est/(sd(x)*sqrt(n))
> se.slope
[1] 0.4284971
> 
> # and the standard error of the intercept
> 
> se.int <- se.slope*sqrt(sum(x*x)/n)
> se.int
[1] 0.2962523
>
```

#### 5. Confidence Intervals for Linear Regression

* First, we consider confidence intervals for the parameter estimates:

```R
# calculate critical value of t for confidence intervals

tval.lookup <- qt(p=c(0.025,0.975),df=nc-2)
tval.lookup
tval <- tval.lookup[2]
tval

# calculate 95% confidence intervals for
# the intercept and slope terms

lcl.ahat <- ahat-tval*se.ahat
lcl.ahat
ucl.ahat <- ahat+tval*se.ahat
ucl.ahat

lcl.bhat <- bhat-tval*se.bhat
lcl.bhat
ucl.bhat <- bhat+tval*se.bhat
ucl.bhat
```

* Output:

```Rout
> # calculate critical value of t for confidence intervals
> 
> tval.lookup <- qt(p=c(0.025,0.975),df=nc-2)
> tval.lookup
[1] -1.962344  1.962344
> tval <- tval.lookup[2]
> tval
[1] 1.962344
> 
> # calculate 95% confidence intervals for
> # the intercept and slope terms
> 
> lcl.ahat <- ahat-tval*se.ahat
> lcl.ahat
(Intercept) 
   49.61329 
> ucl.ahat <- ahat+tval*se.ahat
> ucl.ahat
(Intercept) 
   50.77657 
> 
> lcl.bhat <- bhat-tval*se.bhat
> lcl.bhat
        x 
0.1376592 
> ucl.bhat <- bhat+tval*se.bhat
> ucl.bhat
       x 
1.820218 
>
```

* Next, we consider the confidence intervals for the expected values of y conditional on x:

```R
# 95% confidence interval for E(y|x=0) and E(y|x=1)
# preliminary calculations

# standard error of E(y|x=0)

xstar <- 0
sey.pt1 <- (xstar-xbar)^2
sey.pt2 <- sum(x-xbar^2)
sey.x0 <- sqrt(sigmahat^2*(1/nc+(sey.pt1/sey.pt2)))
sey.x0

# standard error of E(y|x=1)

xstar <- 1
sey.pt1 <- (xstar-xbar)^2
sey.pt2 <- sum(x-xbar^2)
sey.x1 <- sqrt(sigmahat^2*(1/nc+(sey.pt1/sey.pt2)))
sey.x1

# remove labels from ahat and bhat

attr(ahat,"names") <- NULL
attr(bhat,"names") <- NULL

# 95% confidence limits for E(y|x=0)
# final calculations

xstar <- 0
ahat+bhat*xstar
lcl.eyx0 <- (ahat+bhat*xstar)-tval*sey.x0
lcl.eyx0
ucl.eyx0 <- (ahat+bhat*xstar)+tval*sey.x0
ucl.eyx0

xstar <- 1
ahat+bhat*xstar
lcl.eyx1 <- (ahat+bhat*xstar)-tval*sey.x1
lcl.eyx1
ucl.eyx1 <- (ahat+bhat*xstar)+tval*sey.x1
ucl.eyx1
```

* Output:
  
```Rout
> # 95% confidence interval for E(y|x=0) and E(y|x=1)
> # preliminary calculations
> 
> # standard error of E(y|x=0)
> 
> xstar <- 0
> sey.pt1 <- (xstar-xbar)^2
> sey.pt2 <- sum(x-xbar^2)
> sey.x0 <- sqrt(sigmahat^2*(1/nc+(sey.pt1/sey.pt2)))
> sey.x0
[1] 0.2964006
> 
> # standard error of E(y|x=1)
> 
> xstar <- 1
> sey.pt1 <- (xstar-xbar)^2
> sey.pt2 <- sum(x-xbar^2)
> sey.x1 <- sqrt(sigmahat^2*(1/nc+(sey.pt1/sey.pt2)))
> sey.x1
[1] 0.3097422
> 
> # remove labels from ahat and bhat
> 
> attr(ahat,"names") <- NULL
> attr(bhat,"names") <- NULL
> 
> # 95% confidence limits for E(y|x=0)
> # final calculations
> 
> xstar <- 0
> ahat+bhat*xstar
[1] 50.19493
> lcl.eyx0 <- (ahat+bhat*xstar)-tval*sey.x0
> lcl.eyx0
[1] 49.61329
> ucl.eyx0 <- (ahat+bhat*xstar)+tval*sey.x0
> ucl.eyx0
[1] 50.77657
> 
> xstar <- 1
> ahat+bhat*xstar
[1] 51.17387
> lcl.eyx1 <- (ahat+bhat*xstar)-tval*sey.x1
> lcl.eyx1
[1] 50.56605
> ucl.eyx1 <- (ahat+bhat*xstar)+tval*sey.x1
> ucl.eyx1
[1] 51.78169
> 
```

* R gives us a way to verify our calculations:

```R
# check our work

predict(M,newdata=data.frame(x=c(0,1)),
  level=0.95,interval="confidence")
```

* Output:

```Rout
> # check our work
> 
> predict(M,newdata=data.frame(x=c(0,1)),
+   level=0.95,interval="confidence")
       fit      lwr      upr
1 50.19493 49.61329 50.77657
2 51.17387 50.56605 51.78169
>
```

#### 6. Repeated Sampling

* We now study the behavior of the linear regression estimator when we draw thousands of samples from the population with known parameter values.
  
```R
# repeated sampling process

a.rs <- vector()
b.rs <- vector()
ase.rs <- vector()
bse.rs <- vector()
lcl95x0.rs <- vector()
ucl95x0.rs <- vector()
lcl95x1.rs <- vector()
ucl95x1.rs <- vector()

for(i in 1:10000){
  e <- rnorm(n=nc,mean=0,sd=7)
  y.rs <- 50+x+e
  m.rs <- lm(y.rs~1+x)
  a.rs[i] <- coef(m.rs)[1]
  b.rs[i] <- coef(m.rs)[2]
  ase.rs[i] <- sqrt(vcov(m.rs)[1,1])
  bse.rs[i] <- sqrt(vcov(m.rs)[2,2])
  confint <- predict(m.rs,
    newdata=data.frame(x=c(0,1)),
    level=0.95,interval="confidence")
  lcl95x0.rs[i] <- confint[1,2]
  ucl95x0.rs[i] <- confint[1,3]
  lcl95x1.rs[i] <- confint[2,2]
  ucl95x1.rs[i] <- confint[2,3]
  }      

# sampling distribution of parameter estimates
# first look at the histograms

par(mfrow=c(1,2))
hist(a.rs)
hist(b.rs)

# let's review some descriptive statistics

mean(a.rs)
median(a.rs)
sd(a.rs)
mean(ase.rs)

mean(b.rs)
median(b.rs)
sd(b.rs)
mean(bse.rs)
```

* Output:

```Rout
> # repeated sampling process
> 
> a.rs <- vector()
> b.rs <- vector()
> ase.rs <- vector()
> bse.rs <- vector()
> lcl95x0.rs <- vector()
> ucl95x0.rs <- vector()
> lcl95x1.rs <- vector()
> ucl95x1.rs <- vector()
> 
> for(i in 1:10000){
+   e <- rnorm(n=nc,mean=0,sd=7)
+   y.rs <- 50+x+e
+   m.rs <- lm(y.rs~1+x)
+   a.rs[i] <- coef(m.rs)[1]
+   b.rs[i] <- coef(m.rs)[2]
+   ase.rs[i] <- sqrt(vcov(m.rs)[1,1])
+   bse.rs[i] <- sqrt(vcov(m.rs)[2,2])
+   confint <- predict(m.rs,
+     newdata=data.frame(x=c(0,1)),
+     level=0.95,interval="confidence")
+   lcl95x0.rs[i] <- confint[1,2]
+   ucl95x0.rs[i] <- confint[1,3]
+   lcl95x1.rs[i] <- confint[2,2]
+   ucl95x1.rs[i] <- confint[2,3]
+   }      
> 
> # sampling distribution of parameter estimates
> # first look at the histograms
> 
> par(mfrow=c(1,2))
> hist(a.rs)
> hist(b.rs)
> 
> # let's review some descriptive statistics
> 
> mean(a.rs)
[1] 50.00238
> median(a.rs)
[1] 50.00184
> sd(a.rs)
[1] 0.3065062
> mean(ase.rs)
[1] 0.3063913
> 
> mean(b.rs)
[1] 1.00105
> median(b.rs)
[1] 0.9956601
> sd(b.rs)
[1] 0.4433307
> mean(bse.rs)
[1] 0.443162
> 
```

<p align="center">
<img src="/gfiles/h2.png" width="600px">
</p>

* Now, we examine the coverage of our 95% confidence interval estimators:

```R
# calculate 95% confidence intervals for 
# the slope and intercept terms in each sample

lcla.rs <- a.rs-tval*ase.rs
ucla.rs <- a.rs+tval*ase.rs
ahit <- ifelse(lcla.rs<50 & ucla.rs>50,1,0)
mean(ahit)

lclb.rs <- b.rs-tval*bse.rs
uclb.rs <- b.rs+tval*bse.rs
bhit <- ifelse(lclb.rs<1 & uclb.rs>1,1,0)
mean(bhit)

# now check 95% confidence interval 
# coverage for E(y|x=0) and E(y|x=1)

eyx0hit <- ifelse(lcl95x0.rs<50 & ucl95x0.rs>50,1,0)
mean(eyx0hit)

eyx1hit <- ifelse(lcl95x1.rs<51 & ucl95x1.rs>51,1,0)
mean(eyx1hit)
```

* Output:

```Rout
> # calculate 95% confidence intervals for 
> # the slope and intercept terms in each sample
> 
> lcla.rs <- a.rs-tval*ase.rs
> ucla.rs <- a.rs+tval*ase.rs
> ahit <- ifelse(lcla.rs<50 & ucla.rs>50,1,0)
> mean(ahit)
[1] 0.9499
> 
> lclb.rs <- b.rs-tval*bse.rs
> uclb.rs <- b.rs+tval*bse.rs
> bhit <- ifelse(lclb.rs<1 & uclb.rs>1,1,0)
> mean(bhit)
[1] 0.9515
> 
> # now check 95% confidence interval 
> # coverage for E(y|x=0) and E(y|x=1)
> 
> eyx0hit <- ifelse(lcl95x0.rs<50 & ucl95x0.rs>50,1,0)
> mean(eyx0hit)
[1] 0.9499
> 
> eyx1hit <- ifelse(lcl95x1.rs<51 & ucl95x1.rs>51,1,0)
> mean(eyx1hit)
[1] 0.9498
> 
```

#### 7. Introduction to Maximum Likelihood Estimation

* Likelihood corresponds to the probability of the data looking the way they do, conditional on a particular set of parameter values - p(d|m) where d is the data and m is the model.
* Recall our original model, M:

```R
summary(M)
logLik(M)
```

* Here is the regression output:

```Rout
> summary(M)

Call:
lm(formula = y ~ 1 + x)

Residuals:
     Min       1Q   Median       3Q      Max 
-19.9040  -4.4970  -0.0656   4.5976  24.7341 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  50.1949     0.2964 169.348   <2e-16 ***
x             0.9789     0.4287   2.283   0.0226 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 6.772 on 998 degrees of freedom
Multiple R-squared:  0.005197,	Adjusted R-squared:  0.004201 
F-statistic: 5.214 on 1 and 998 DF,  p-value: 0.02261

> logLik(M)
'log Lik.' -3330.728 (df=3)
>
```

* Next, we illustrate a grid-search maximum likelihood method to illustrate the key ideas:

```R
intvalues <- seq(from=1,to=100,by=1)
slopevalues <- seq(from=-1,to=1,by=0.1)
sigvalues <- seq(from=1,to=10,by=0.5)

m <- expand.grid(intvalues,slopevalues,sigvalues)
names(m) <- c("intvalues","slopevalues","sigvalues")
dim(m)
head(m,n=10)

log.likelihood <- vector()

for(i in 1:nrow(m)){
  mu <- m$intvalues[i]+m$slopevalues[i]*x
  sig <- m$sigvalues[i]
  pt1 <- 1/sqrt(2*pi*sig^2)
  pt2 <- -1*(y-mu)^2
  pt3 <- 2*sig^2
  lpdf <- log(pt1*exp(pt2/pt3))
  log.likelihood[i] <- sum(lpdf)
  }

ml <- cbind(m,log.likelihood)
subset(ml,log.likelihood==max(log.likelihood))
sorted.ml <- ml[order(log.likelihood,decreasing=T),] 
head(sorted.ml,n=10)
```

* Output:

```Rout
> intvalues <- seq(from=1,to=100,by=1)
> slopevalues <- seq(from=-1,to=1,by=0.1)
> sigvalues <- seq(from=1,to=10,by=0.5)
> 
> m <- expand.grid(intvalues,slopevalues,sigvalues)
> names(m) <- c("intvalues","slopevalues","sigvalues")
> dim(m)
[1] 39900     3
> head(m,n=10)
   intvalues slopevalues sigvalues
1          1          -1         1
2          2          -1         1
3          3          -1         1
4          4          -1         1
5          5          -1         1
6          6          -1         1
7          7          -1         1
8          8          -1         1
9          9          -1         1
10        10          -1         1
> 
> log.likelihood <- vector()
> 
> for(i in 1:nrow(m)){
+   mu <- m$intvalues[i]+m$slopevalues[i]*x
+   sig <- m$sigvalues[i]
+   pt1 <- 1/sqrt(2*pi*sig^2)
+   pt2 <- -1*(y-mu)^2
+   pt3 <- 2*sig^2
+   lpdf <- log(pt1*exp(pt2/pt3))
+   log.likelihood[i] <- sum(lpdf)
+   }
> 
> ml <- cbind(m,log.likelihood)
> subset(ml,log.likelihood==max(log.likelihood))
      intvalues slopevalues sigvalues log.likelihood
27250        50           1         7      -3332.216
> sorted.ml <- ml[order(log.likelihood,decreasing=T),] 
> head(sorted.ml,n=10)
      intvalues slopevalues sigvalues log.likelihood
27250        50         1.0       7.0      -3332.216
27150        50         0.9       7.0      -3332.434
27050        50         0.8       7.0      -3332.750
25150        50         1.0       6.5      -3332.776
25050        50         0.9       6.5      -3333.029
26950        50         0.7       7.0      -3333.164
24950        50         0.8       6.5      -3333.396
26850        50         0.6       7.0      -3333.675
24850        50         0.7       6.5      -3333.875
26750        50         0.5       7.0      -3334.283
>
```

#### Assignment #1: Due End of Day on Friday 9/13/24

*Instructions*: Please complete each of the tasks outlined below. When you are finished, please email a readable pdf copy of your work where your responses to each of the tasks and questions are clearly indicated. You are also welcome to submit a paper copy if you prefer (but any submissions arriving after 5pm on Friday 9/13 should be sent via email so there is a time record of the submission). Also, if you choose to submit a paper copy and I am not in the office when you submit, you should put your paper copy in an interoffice mail envelope and slide it under my office door. Please show all calculations in an organized and easy to read format. This is to help me grade your work but it is also so you can go back to your notes at a later date and be able to understand what you did and why you did it. A couple of points to keep in mind about late submissions: (1) if your assignment is submitted late (after 11:59pm on Friday 9/13/24), there will be an automatic 10 point deduction for a late submission; (2) if your assignment is submitted more than 24 hours late, there will be an automatic 20 point deduction; (3) if your assignment is submitted more than 48 hours late, the maximum available score on the assignment will be a 50. Please double check that I can read your pdf file before you submit it. If the file is corrupt or has a virus or I am unable to read it, it will be treated the same as a late submission. Once I receive your submission (which may not be immediately after you send it), I will write to you and confirm that I have received it and can open your file. I will also confirm receipt of paper submissions. If you choose to ask me a clarifying question, I will determine whether I think it is proper to answer the question. If I decide to answer the question, I will post both the question and my response below the assignment for all students to see. I will not be able to answer any clarifying questions after class on Thursday 9/12. I suggest you not submit your assignment until after that time. The point values for each task are listed next to the task. Finally, the work you submit for this project must be your own work. Good luck!

1. You already know that the sampling distribution of a sample mean gets smaller when the sample size increases. Design a simulation where you demonstrate what happens to the sampling distribution of the sample median when the sample size increases. Note: please use your UMD student id number to seed your simulation study. (20pts).

2. Consider the following dataset:

```R
set.seed(your student id)
x <- rnorm(n=300,mean=10,sd=1)
e <- rnorm(n=300,mean=0,sd=8)
y <- 1/3*x+e
d <- data.frame(x,y)
```

* Estimate a regression where *x* is the independent variable and *y* is the dependent variable (10pts).
* Use the variance-covariance matrix of the parameter estimates to calculate the standard error of the intercept and the slope coefficient (10pts).
* Estimate a 80% confidence interval for the slope coefficient (15pts).
* Use the arithmetic formula to estimate a 90% confidence interval for the expected value of *y* when x is equal to 10; confirm your answer with the predict() function (15pts).

3. Suppose we have a state population of 7822 people released on bail. The population failure to appear (FTA) rate is 27% (which would ordinarily not be known to us). Suppose we are able to draw a single random sample of size N = 87 from this population and that 33 of the 87 people in our sample failed to appear. Estimate a 90% confidence interval for the FTA rate in the single sample (20pts). Comment on whether the 90% confidence interval in the sample includes the true population parameter value (10pts). 

### Lesson 3 - Thursday 9/12/24

* Introduction to maximum likelihood (linear regression)
* Introduction to maximum likelihood (proportion)
* Inferential issues with proportions
  
#### 7. Introduction to Maximum Likelihood Estimation

* Likelihood corresponds to the probability of the data looking the way they do, conditional on a particular set of parameter values - p(d|m) where d is the data and m is the model.
* Recall our original model, M:

```R
summary(M)
logLik(M)
```

* Here is the regression output:

```Rout
> summary(M)

Call:
lm(formula = y ~ 1 + x)

Residuals:
     Min       1Q   Median       3Q      Max 
-19.9040  -4.4970  -0.0656   4.5976  24.7341 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  50.1949     0.2964 169.348   <2e-16 ***
x             0.9789     0.4287   2.283   0.0226 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 6.772 on 998 degrees of freedom
Multiple R-squared:  0.005197,	Adjusted R-squared:  0.004201 
F-statistic: 5.214 on 1 and 998 DF,  p-value: 0.02261

> logLik(M)
'log Lik.' -3330.728 (df=3)
>
```

* Next, we illustrate a grid-search maximum likelihood method to illustrate the key ideas:

```R
intvalues <- seq(from=1,to=100,by=1)
slopevalues <- seq(from=-1,to=1,by=0.1)
sigvalues <- seq(from=1,to=10,by=0.5)

m <- expand.grid(intvalues,slopevalues,sigvalues)
names(m) <- c("intvalues","slopevalues","sigvalues")
dim(m)
head(m,n=10)

log.likelihood <- vector()

for(i in 1:nrow(m)){
  mu <- m$intvalues[i]+m$slopevalues[i]*x
  sig <- m$sigvalues[i]
  pt1 <- 1/sqrt(2*pi*sig^2)
  pt2 <- -1*(y-mu)^2
  pt3 <- 2*sig^2
  lpdf <- log(pt1*exp(pt2/pt3))
  log.likelihood[i] <- sum(lpdf)
  }

ml <- cbind(m,log.likelihood)
subset(ml,log.likelihood==max(log.likelihood))
sorted.ml <- ml[order(log.likelihood,decreasing=T),] 
head(sorted.ml,n=10)
```

* Output:

```Rout
> intvalues <- seq(from=1,to=100,by=1)
> slopevalues <- seq(from=-1,to=1,by=0.1)
> sigvalues <- seq(from=1,to=10,by=0.5)
> 
> m <- expand.grid(intvalues,slopevalues,sigvalues)
> names(m) <- c("intvalues","slopevalues","sigvalues")
> dim(m)
[1] 39900     3
> head(m,n=10)
   intvalues slopevalues sigvalues
1          1          -1         1
2          2          -1         1
3          3          -1         1
4          4          -1         1
5          5          -1         1
6          6          -1         1
7          7          -1         1
8          8          -1         1
9          9          -1         1
10        10          -1         1
> 
> log.likelihood <- vector()
> 
> for(i in 1:nrow(m)){
+   mu <- m$intvalues[i]+m$slopevalues[i]*x
+   sig <- m$sigvalues[i]
+   pt1 <- 1/sqrt(2*pi*sig^2)
+   pt2 <- -1*(y-mu)^2
+   pt3 <- 2*sig^2
+   lpdf <- log(pt1*exp(pt2/pt3))
+   log.likelihood[i] <- sum(lpdf)
+   }
> 
> ml <- cbind(m,log.likelihood)
> subset(ml,log.likelihood==max(log.likelihood))
      intvalues slopevalues sigvalues log.likelihood
27250        50           1         7      -3332.216
> sorted.ml <- ml[order(log.likelihood,decreasing=T),] 
> head(sorted.ml,n=10)
      intvalues slopevalues sigvalues log.likelihood
27250        50         1.0       7.0      -3332.216
27150        50         0.9       7.0      -3332.434
27050        50         0.8       7.0      -3332.750
25150        50         1.0       6.5      -3332.776
25050        50         0.9       6.5      -3333.029
26950        50         0.7       7.0      -3333.164
24950        50         0.8       6.5      -3333.396
26850        50         0.6       7.0      -3333.675
24850        50         0.7       6.5      -3333.875
26750        50         0.5       7.0      -3334.283
>
```

#### 8. Maximum Likelihood Estimation for a Proportion

* The observed data come from Bushway, Phillips, and Cook (2012:442; [link](https://www.degruyter.com/document/doi/10.1111/j.1468-0475.2012.00578.x/html)).

```R
y <- c(rep(0,3),rep(1,10))
y
table(y)
mean(y)
```

* Here is our output:

```Rout
> y <- c(rep(0,3),rep(1,10))
> y
 [1] 0 0 0 1 1 1 1 1 1 1 1 1 1
> table(y)
y
 0  1 
 3 10 
> mean(y)
[1] 0.7692308
>
```

* What is the maximum likelihood estimate of *p*?; where *p* is estimated by the number of times an event occurred (*r*) divided by the number of times an event could have occurred (*N*). In this case, that estimate would be 10/13.
* To find an approximate maximum likelihood estimate of *p* we could use a grid search method like the one we used in #7 above.
* This will require that we code the binomial probability mass function:

```R
N <- 13
r <- 10
r/N

p <- seq(from=0,to=1,by=0.01)
likelihood <- choose(N,r)*p^r*(1-p)^(N-r)
options(scipen=100)
data.frame(p,likelihood)
```

* which gives us the following (long) output:

```Rout
> N <- 13
> r <- 10
> r/N
[1] 0.7692308
> 
> p <- seq(from=0,to=1,by=0.01)
> likelihood <- choose(N,r)*p^r*(1-p)^(N-r)
> options(scipen=100)
> data.frame(p,likelihood)
       p                 likelihood
1   0.00 0.000000000000000000000000
2   0.01 0.000000000000000002775055
3   0.02 0.000000000000002756412539
4   0.03 0.000000000000154132344014
5   0.04 0.000000000002653258996777
6   0.05 0.000000000023946215820313
7   0.06 0.000000000143635601614602
8   0.07 0.000000000649823299439294
9   0.08 0.000000002391274238058169
10  0.09 0.000000007514763278439475
11  0.10 0.000000020849400000000012
12  0.11 0.000000052295329610931190
13  0.12 0.000000120677557092829039
14  0.13 0.000000259631459110000306
15  0.14 0.000000526188974240686383
16  0.15 0.000001012827304467772898
17  0.16 0.000001863818927911930489
18  0.17 0.000003296776638458255086
19  0.18 0.000005630314329094372920
20  0.19 0.000009318737951700382271
21  0.20 0.000014994636800000011917
22  0.21 0.000023520159055568149836
23  0.22 0.000036047624414275319677
24  0.23 0.000054089948609556367566
25  0.24 0.000079601128929198696448
26  0.25 0.000115066766738891601562
27  0.26 0.000163604284183464820154
28  0.27 0.000229072130527423374613
29  0.28 0.000316186873218026408792
30  0.29 0.000430646635212245131909
31  0.30 0.000579258880199999613529
32  0.31 0.000770070069133438860065
33  0.32 0.001012494224193852352528
34  0.33 0.001317436950386961574608
35  0.34 0.001697410991794145399686
36  0.35 0.002166638951503394548703
37  0.36 0.002741138394599944034385
38  0.37 0.003438784196190102643298
39  0.38 0.004279342705658985453188
40  0.39 0.005284472088961424292297
41  0.40 0.006477683097600002437577
42  0.41 0.007884254510875005755866
43  0.42 0.009531097621522058305210
44  0.43 0.011446564397965984169470
45  0.44 0.013660194372285170208436
46  0.45 0.016202395883680012489414
47  0.46 0.019104058063455893468063
48  0.47 0.022396090888238712190983
49  0.48 0.026108891760260724557163
50  0.49 0.030271738401603350693270
51  0.50 0.034912109375000000000000
52  0.51 0.040054935265759247786654
53  0.52 0.045721785472557813223560
54  0.53 0.051929997650269794917666
55  0.54 0.058691759112187109892478
56  0.55 0.066013151913585171870480
57  0.56 0.073893175879847339260209
58  0.57 0.082322766480649178788553
59  0.58 0.091283827150982640996624
60  0.59 0.100748298377107803336372
61  0.60 0.110677288550399974265126
62  0.61 0.121020294186021296067857
63  0.62 0.131714539539232816656167
64  0.63 0.142684467853608692999856
65  0.64 0.153841418356159997937738
66  0.65 0.165083524577675833100443
67  0.66 0.176295870514529273709314
68  0.67 0.187350941441025264921905
69  0.68 0.198109405696806906149732
70  0.69 0.208421262366362686213606
71  0.70 0.218127387277800038889453
72  0.71 0.227061506001558721656863
73  0.72 0.235052617336945240955615
74  0.73 0.241927883929317605327114
75  0.74 0.247515997940430942936274
76  0.75 0.251651018857955932617188
77  0.76 0.254176667317908611121169
78  0.77 0.254951042946475026074182
79  0.78 0.253851715405868760822017
80  0.79 0.250781115731452675099433
81  0.80 0.245672129331199973201194
82  0.81 0.238493762317300350694893
83  0.82 0.229256718762878120010384
84  0.83 0.218018687608843075853571
85  0.84 0.204889093845281006212034
86  0.85 0.190033018789882574006711
87  0.86 0.173673938286064311053991
88  0.87 0.156094864919875092601487
89  0.88 0.137637410351654609907968
90  0.89 0.118698205988605046123929
91  0.90 0.099722033868599957440182
92  0.91 0.081190924130916650169887
93  0.92 0.063608370128874824889209
94  0.93 0.047477696369674228515922
95  0.94 0.033273487288326450417308
96  0.95 0.021404845577771974829417
97  0.96 0.012169096569188462761413
98  0.97 0.005694389107882650083448
99  0.98 0.001869462582158711765404
100 0.99 0.000258653273452518785418
101 1.00 0.000000000000000000000000
>
```

* Note that we can also graph the function:

```R
plot(x=p,y=likelihood,type="l",lty=1,lwd=2)
```

which gives us the following chart:

<p align="center">
<img src="/gfiles/like-plot.png" width="500px">
</p>

* Note that we could get a more detailed set of results by estimating a logistic regression model with only an intercept term.

```R
M <- glm(y~1,family="binomial")
summary(M)
```

* Here is the model output:

```Rout
> M <- glm(y~1,family="binomial")
> summary(M)

Call:
glm(formula = y ~ 1, family = "binomial")

Coefficients:
            Estimate Std. Error z value Pr(>|z|)  
(Intercept)   1.2040     0.6583   1.829   0.0674 .
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 14.045  on 12  degrees of freedom
Residual deviance: 14.045  on 12  degrees of freedom
AIC: 16.045

Number of Fisher Scoring iterations: 4

>
```

* How do we interpret this estimate?

```R
intest <- coef(M)[1]
intest
phat <- exp(intest)/(1+exp(intest))
attr(phat,"names") <- NULL
phat
```

* Here is our output:

```Rout
> intest <- coef(M)[1]
> intest
(Intercept) 
   1.203973 
> phat <- exp(intest)/(1+exp(intest))
> attr(phat,"names") <- NULL
> phat
[1] 0.7692308
>
```

* Notice that there is a test of the hypothesis that the intercept is equal to zero.
* What would an intercept of 0 imply about the probability distribution of the outcome?

```R
exp(0)/(1+exp(0))
```

* So, as we can see below, an intercept of 0 with no covariates implies that the outcome is equally distributed between the 0's and 1's:

```Rout
> exp(0)/(1+exp(0))
[1] 0.5
>
```

* So, if we were using a 95% confidence level for our hypothesis test, we would fail to reject the hypothesis that the intercept is equal to zero (or, equivalently that p is equal to 0.5).
* Note the relationship between "logistic metric" and "probability metric":

```R
logit <- seq(from=-3,to=3,by=0.1)
py <- exp(logit)/(1+exp(logit))
plot(x=logit,y=py,type="l",lty=1,lwd=2)
```

<p align="center">
<img src="/gfiles/logit-plot.png" width="500px">
</p>

* This raises questions about inference for proportions.

#### 9. Inference about Proportions

* A simple approach for calculating a 95% confidence interval for *p* would be to use the information in the logistic regression model:

```R
logit.lcl <- 1.204-1.96*0.6583
logit.ucl <- 1.204+1.96*0.6583
exp(logit.lcl)/(1+exp(logit.lcl))
exp(logit.ucl)/(1+exp(logit.ucl))
```

which yields the following output:

```Rout
> logit.lcl <- 1.204-1.96*0.6583
> logit.ucl <- 1.204+1.96*0.6583
> exp(logit.lcl)/(1+exp(logit.lcl))
[1] 0.4784464
> exp(logit.ucl)/(1+exp(logit.ucl))
[1] 0.923739
>
```

### Lesson 4 - Thursday 9/19/24

* First assignments returned; next assignment will be distributed next week (due on 10/3).
* Proportions (continued)
* Contingency tables

#### 10. Confidence Intervals for Proportions

* At the the end of last week's class, we considered how to use a logistic regression model with just an intercept to calculate a (95%) confidence interval for a proportion.
* Recall that the inference was based on a sample proportion of r = 10 countercylical movements in robbery rates across N = 13 business cycles; thus the sample estimate of p = r/N = 10/13 = 0.769.
* The logistic regression-based 95% confidence interval of p was estimated to be [0.479,0.924] which includes the value p = 0.5.
* Approach #2: normal approximation to the binomial (often presented in statistics textbooks):

```R
r <- 10
N <- 13
p <- r/N
p
vp <- p*(1-p)/N
vp
sp <- sqrt(vp)
sp
mult <- qnorm(0.975)
mult
lcl <- p-mult*sp
lcl
ucl <- p+mult*sp
ucl
```

which gives the following output:

```Rout
> r <- 10
> N <- 13
> p <- r/N
> p
[1] 0.7692308
> vp <- p*(1-p)/N
> vp
[1] 0.01365498
> sp <- sqrt(vp)
> sp
[1] 0.1168545
> mult <- qnorm(0.975)
> mult
[1] 1.959964
> lcl <- p-mult*sp
> lcl
[1] 0.5402001
> ucl <- p+mult*sp
> ucl
[1] 0.9982615
> 
```

* Notice that this confidence interval does not include 0.5. Also notice that there is nothing to prevent this confidence interval from having a lower limit that is less than zero or an upper limit that is greater than 1.0. For example, here is a 99% confidence interval:

```R
r <- 10
N <- 13
p <- r/N
p
vp <- p*(1-p)/N
vp
sp <- sqrt(vp)
sp
mult <- qnorm(0.995)
mult
lcl <- p-mult*sp
lcl
ucl <- p+mult*sp
ucl
```

which gives us the following interval:

```Rout
> r <- 10
> N <- 13
> p <- r/N
> p
[1] 0.7692308
> vp <- p*(1-p)/N
> vp
[1] 0.01365498
> sp <- sqrt(vp)
> sp
[1] 0.1168545
> mult <- qnorm(0.995)
> mult
[1] 2.575829
> lcl <- p-mult*sp
> lcl
[1] 0.4682334
> ucl <- p+mult*sp
> ucl
[1] 1.070228
> 
```

* Now, the lower limit is below 0.5 (as we would expect based on the hypothesis test reported in the article and the logistic regression results) but the upper limit is well over 1.0 which can't be right.

* Still another approach would be to rely directly on the binomial distribution to calculate a confidence interval. This is the so-called Clopper-Pearson confidence interval for p which relies on the *beta distribution*:

```R
r <- 10
N <- 13
lcl <- qbeta(0.025,shape1=r,shape2=N-r+1)
lcl
ucl <- qbeta(0.975,shape1=r+1,shape2=N-r)
ucl
```

and the lower and upper confidence limits are estimated to be:

```Rout
> r <- 10
> N <- 13
> lcl <- qbeta(0.025,shape1=r,shape2=N-r+1)
> lcl
[1] 0.4618685
> ucl <- qbeta(0.975,shape1=r+1,shape2=N-r)
> ucl
[1] 0.9496189
```

* Notice that this 95% interval is wider than the 95% logistic regression interval (signaling greater uncertainty). Unlike the normal approximation interval, this interval will always respect the [0,1] boundaries for a proportion.

* Another interval we will consider is the 95% Bayesian credibility interval for the proportion based on the Jeffreys prior. It has been established that this interval has good confidence interval properties in small samples. And, like the Clopper-Pearson interval, it relies on the beta distribution:

```R
r <- 10
N <- 13
lcl <- qbeta(0.025,shape1=r+1/2,shape2=N-r+1/2)
lcl
ucl <- qbeta(0.975,shape1=r+1/2,shape2=N-r+1/2)
ucl
```

* Here is the output:

```Rout
> r <- 10
> N <- 13
> lcl <- qbeta(0.025,shape1=r+1/2,shape2=N-r+1/2)
> lcl
[1] 0.5026279
> ucl <- qbeta(0.975,shape1=r+1/2,shape2=N-r+1/2)
> ucl
[1] 0.9303433
> 
```

* Notice that this interval has a shorter width than either the Clopper-Pearson interval or the logistic regression interval. Notice also that both the logistic regression and the Clopper-Pearson intervals include 0.5 within the range of uncertainty. In contrast, the normal approximation interval excludes 0.5 but is wider than all of the other intervals. 

* Finally, we will consider an interval based on the (percentile) bootstrap. When we use the bootstrap we draw samples repeatedly from the sample (with replacement). Here is how it works for this problem:

```R
set.seed(11)
r <- 10
N <- 13
y <- c(rep(0,N-r),rep(1,r))
y
mean(y)

# draw a bootstrap sample

sb <- sample(1:N,size=N,replace=T)
sb
ysb <- y[sb]
mean(ysb)

# draw another bootstrap sample

sb <- sample(1:N,size=N,replace=T)
sb
ysb <- y[sb]
mean(ysb)

# now let's draw a bunch of bootstrap samples

pb <- vector()

for(i in 1:10000){
  b <- sample(1:N,size=N,replace=T)
  yb <- y[b]
  pb[i] <- mean(yb)
  }

lcl.boot <- quantile(pb,0.025)
lcl.boot
ucl.boot <- quantile(pb,0.975)
ucl.boot
```

* Here is our output:

```Rout
> set.seed(11)
> r <- 10
> N <- 13
> y <- c(rep(0,N-r),rep(1,r))
> y
 [1] 0 0 0 1 1 1 1 1 1 1 1 1 1
> mean(y)
[1] 0.7692308
> 
> # draw a bootstrap sample
> 
> sb <- sample(1:N,size=N,replace=T)
> sb
 [1] 10  2  8  9  1  5 12  6 12  5  6  7  5
> ysb <- y[sb]
> mean(ysb)
[1] 0.8461538
> 
> # draw another bootstrap sample
> 
> sb <- sample(1:N,size=N,replace=T)
> sb
 [1]  3 13 11  7 13  2  1  8  7  6  3  3 11
> ysb <- y[sb]
> mean(ysb)
[1] 0.6153846
> 
> # now let's draw a bunch of bootstrap samples
> 
> pb <- vector()
> 
> for(i in 1:10000){
+   b <- sample(1:N,size=N,replace=T)
+   yb <- y[b]
+   pb[i] <- mean(yb)
+   }
> 
> lcl.boot <- quantile(pb,0.025)
> lcl.boot
     2.5% 
0.5384615 
> ucl.boot <- quantile(pb,0.975)
> ucl.boot
97.5% 
    1 
> 
```

* So, our bootstrap-based 95% confidence interval is [0.538,1.0]. Notice that this interval also obeys the constraint that a proportion cannot be lower than zero or greater than one.
* Now we have five different ways of calculating a confidence interval for this proportion. Which approach works the best in this situation? Let's find out.

* Here is a simulation:

```R
set.seed(333)
N <- 13
pop.p <- 10/13
pop.p
mult <- qnorm(0.975)
mult

lcl.logit <- vector()
ucl.logit <- vector()
lcl.normal <- vector()
ucl.normal <- vector()
lcl.cp <- vector()
ucl.cp <- vector()
lcl.jp <- vector()
ucl.jp <- vector()
lcl.b <- vector()
ucl.b <- vector()

for(i in 1:3000) {
  r <- rbinom(n=1,size=N,p=pop.p)
  y <- c(rep(0,N-r),rep(1,r))

  # logistic regression

  M <- glm(y~1,family="binomial")
  int <- coef(M)[1]
  se.int <- sqrt(vcov(M)[1,1])
  lcl.logit[i] <- exp(int-mult*se.int)/(1+exp(int-mult*se.int))
  ucl.logit[i] <- exp(int+mult*se.int)/(1+exp(int+mult*se.int))

  # normal approximation to the binomial

  phat <- mean(y)
  se.phat <- sqrt(phat*(1-phat)/N)
  lcl.normal[i] <- phat-mult*se.phat
  ucl.normal[i] <- phat+mult*se.phat

  # Clopper-Pearson

  lcl.cp[i] <- qbeta(0.025,shape1=r,shape2=N-r+1)
  ucl.cp[i] <- qbeta(0.975,shape1=r+1,shape2=N-r)

  # Jeffreys Prior

  lcl.jp[i] <- qbeta(0.025,shape1=r+1/2,shape2=N-r+1/2)
  ucl.jp[i] <- qbeta(0.975,shape1=r+1/2,shape2=N-r+1/2)

  # bootstrap

  pb <- vector()

  for(j in 1:3000){
    b <- sample(1:N,size=N,replace=T)
    yb <- y[b]
    pb[j] <- mean(yb)
    }

  lcl.b[i] <- quantile(pb,0.025)
  ucl.b[i] <- quantile(pb,0.975)
  }

table(is.na(lcl.logit))
table(is.na(ucl.logit))
ucl.logit[is.na(ucl.logit)] <- 1
lr.trap <- mean(ifelse(lcl.logit<pop.p & ucl.logit>pop.p,1,0))
lr.trap
na.trap <- mean(ifelse(lcl.normal<pop.p & ucl.normal>pop.p,1,0))
na.trap
table(ucl.normal>1)
cp.trap <- mean(ifelse(lcl.cp<pop.p & ucl.cp>pop.p,1,0))
cp.trap
jp.trap <- mean(ifelse(lcl.jp<pop.p & ucl.jp>pop.p,1,0))
jp.trap
b.trap <- mean(ifelse(lcl.b<pop.p & ucl.b>pop.p,1,0))
b.trap
```

* Here is our output:

```Rout
> set.seed(333)
> N <- 13
> pop.p <- 10/13
> pop.p
[1] 0.7692308
> mult <- qnorm(0.975)
> mult
[1] 1.959964
> 
> lcl.logit <- vector()
> ucl.logit <- vector()
> lcl.normal <- vector()
> ucl.normal <- vector()
> lcl.cp <- vector()
> ucl.cp <- vector()
> lcl.jp <- vector()
> ucl.jp <- vector()
> lcl.b <- vector()
> ucl.b <- vector()
> 
> for(i in 1:3000) {
+   r <- rbinom(n=1,size=N,p=pop.p)
+   y <- c(rep(0,N-r),rep(1,r))
+ 
+   # logistic regression
+ 
+   M <- glm(y~1,family="binomial")
+   int <- coef(M)[1]
+   se.int <- sqrt(vcov(M)[1,1])
+   lcl.logit[i] <- exp(int-mult*se.int)/(1+exp(int-mult*se.int))
+   ucl.logit[i] <- exp(int+mult*se.int)/(1+exp(int+mult*se.int))
+ 
+   # normal approximation to the binomial
+ 
+   phat <- mean(y)
+   se.phat <- sqrt(phat*(1-phat)/N)
+   lcl.normal[i] <- phat-mult*se.phat
+   ucl.normal[i] <- phat+mult*se.phat
+ 
+   # Clopper-Pearson
+ 
+   lcl.cp[i] <- qbeta(0.025,shape1=r,shape2=N-r+1)
+   ucl.cp[i] <- qbeta(0.975,shape1=r+1,shape2=N-r)
+ 
+   # Jeffreys Prior
+ 
+   lcl.jp[i] <- qbeta(0.025,shape1=r+1/2,shape2=N-r+1/2)
+   ucl.jp[i] <- qbeta(0.975,shape1=r+1/2,shape2=N-r+1/2)
+ 
+   # bootstrap
+ 
+   pb <- vector()
+ 
+   for(j in 1:3000){
+     b <- sample(1:N,size=N,replace=T)
+     yb <- y[b]
+     pb[j] <- mean(yb)
+     }
+ 
+   lcl.b[i] <- quantile(pb,0.025)
+   ucl.b[i] <- quantile(pb,0.975)
+   }
> 
> table(is.na(lcl.logit))

FALSE 
 3000 
> table(is.na(ucl.logit))

FALSE  TRUE 
 2907    93 
> ucl.logit[is.na(ucl.logit)] <- 1
> lr.trap <- mean(ifelse(lcl.logit<pop.p & ucl.logit>pop.p,1,0))
> lr.trap
[1] 0.9836667
> na.trap <- mean(ifelse(lcl.normal<pop.p & ucl.normal>pop.p,1,0))
> na.trap
[1] 0.821
> table(ucl.normal>1)

FALSE  TRUE 
 1900  1100 
> cp.trap <- mean(ifelse(lcl.cp<pop.p & ucl.cp>pop.p,1,0))
> cp.trap
[1] 0.9836667
> jp.trap <- mean(ifelse(lcl.jp<pop.p & ucl.jp>pop.p,1,0))
> jp.trap
[1] 0.9526667
> b.trap <- mean(ifelse(lcl.b<pop.p & ucl.b>pop.p,1,0))
> b.trap
[1] 0.787
> 
```

* There are some pretty clear patterns here. What conclusions would you draw?
* Now, let's see what happens to all of these methods when we scale up our number of trials by a factor of 7.

```R
set.seed(107)
N <- 13*7
pop.p <- 10/13
pop.p
mult <- qnorm(0.975)
mult

lcl.logit <- vector()
ucl.logit <- vector()
lcl.normal <- vector()
ucl.normal <- vector()
lcl.cp <- vector()
ucl.cp <- vector()
lcl.jp <- vector()
ucl.jp <- vector()
lcl.b <- vector()
ucl.b <- vector()

for(i in 1:3000) {
  r <- rbinom(n=1,size=N,p=pop.p)
  y <- c(rep(0,N-r),rep(1,r))

  # logistic regression

  M <- glm(y~1,family="binomial")
  int <- coef(M)[1]
  se.int <- sqrt(vcov(M)[1,1])
  lcl.logit[i] <- exp(int-mult*se.int)/(1+exp(int-mult*se.int))
  ucl.logit[i] <- exp(int+mult*se.int)/(1+exp(int+mult*se.int))

  # normal approximation to the binomial

  phat <- mean(y)
  se.phat <- sqrt(phat*(1-phat)/N)
  lcl.normal[i] <- phat-mult*se.phat
  ucl.normal[i] <- phat+mult*se.phat

  # Clopper-Pearson

  lcl.cp[i] <- qbeta(0.025,shape1=r,shape2=N-r+1)
  ucl.cp[i] <- qbeta(0.975,shape1=r+1,shape2=N-r)

  # Jeffreys Prior

  lcl.jp[i] <- qbeta(0.025,shape1=r+1/2,shape2=N-r+1/2)
  ucl.jp[i] <- qbeta(0.975,shape1=r+1/2,shape2=N-r+1/2)

  # bootstrap

  pb <- vector()

  for(j in 1:3000){
    b <- sample(1:N,size=N,replace=T)
    yb <- y[b]
    pb[j] <- mean(yb)
    }

  lcl.b[i] <- quantile(pb,0.025)
  ucl.b[i] <- quantile(pb,0.975)
  }

table(is.na(lcl.logit))
table(is.na(ucl.logit))
ucl.logit[is.na(ucl.logit)] <- 1
lr.trap <- mean(ifelse(lcl.logit<pop.p & ucl.logit>pop.p,1,0))
lr.trap
na.trap <- mean(ifelse(lcl.normal<pop.p & ucl.normal>pop.p,1,0))
na.trap
table(ucl.normal>1)
cp.trap <- mean(ifelse(lcl.cp<pop.p & ucl.cp>pop.p,1,0))
cp.trap
jp.trap <- mean(ifelse(lcl.jp<pop.p & ucl.jp>pop.p,1,0))
jp.trap
b.trap <- mean(ifelse(lcl.b<pop.p & ucl.b>pop.p,1,0))
b.trap
```

* And, here is our output:

```Rout
> set.seed(107)
> N <- 13*7
> pop.p <- 10/13
> pop.p
[1] 0.7692308
> mult <- qnorm(0.975)
> mult
[1] 1.959964
> 
> lcl.logit <- vector()
> ucl.logit <- vector()
> lcl.normal <- vector()
> ucl.normal <- vector()
> lcl.cp <- vector()
> ucl.cp <- vector()
> lcl.jp <- vector()
> ucl.jp <- vector()
> lcl.b <- vector()
> ucl.b <- vector()
> 
> for(i in 1:3000) {
+   r <- rbinom(n=1,size=N,p=pop.p)
+   y <- c(rep(0,N-r),rep(1,r))
+ 
+   # logistic regression
+ 
+   M <- glm(y~1,family="binomial")
+   int <- coef(M)[1]
+   se.int <- sqrt(vcov(M)[1,1])
+   lcl.logit[i] <- exp(int-mult*se.int)/(1+exp(int-mult*se.int))
+   ucl.logit[i] <- exp(int+mult*se.int)/(1+exp(int+mult*se.int))
+ 
+   # normal approximation to the binomial
+ 
+   phat <- mean(y)
+   se.phat <- sqrt(phat*(1-phat)/N)
+   lcl.normal[i] <- phat-mult*se.phat
+   ucl.normal[i] <- phat+mult*se.phat
+ 
+   # Clopper-Pearson
+ 
+   lcl.cp[i] <- qbeta(0.025,shape1=r,shape2=N-r+1)
+   ucl.cp[i] <- qbeta(0.975,shape1=r+1,shape2=N-r)
+ 
+   # Jeffreys Prior
+ 
+   lcl.jp[i] <- qbeta(0.025,shape1=r+1/2,shape2=N-r+1/2)
+   ucl.jp[i] <- qbeta(0.975,shape1=r+1/2,shape2=N-r+1/2)
+ 
+   # bootstrap
+ 
+   pb <- vector()
+ 
+   for(j in 1:3000){
+     b <- sample(1:N,size=N,replace=T)
+     yb <- y[b]
+     pb[j] <- mean(yb)
+     }
+ 
+   lcl.b[i] <- quantile(pb,0.025)
+   ucl.b[i] <- quantile(pb,0.975)
+   }
> 
> table(is.na(lcl.logit))

FALSE 
 3000 
> table(is.na(ucl.logit))

FALSE 
 3000 
> ucl.logit[is.na(ucl.logit)] <- 1
> lr.trap <- mean(ifelse(lcl.logit<pop.p & ucl.logit>pop.p,1,0))
> lr.trap
[1] 0.9346667
> na.trap <- mean(ifelse(lcl.normal<pop.p & ucl.normal>pop.p,1,0))
> na.trap
[1] 0.9313333
> table(ucl.normal>1)

FALSE 
 3000 
> cp.trap <- mean(ifelse(lcl.cp<pop.p & ucl.cp>pop.p,1,0))
> cp.trap
[1] 0.9666667
> jp.trap <- mean(ifelse(lcl.jp<pop.p & ucl.jp>pop.p,1,0))
> jp.trap
[1] 0.9526667
> b.trap <- mean(ifelse(lcl.b<pop.p & ucl.b>pop.p,1,0))
> b.trap
[1] 0.9253333
> 
```

### 11. Contingency Tables

* Consider the following real dataset from the Minneapolis Domestic Violence Experiment:

```R
# arrest group

na <- 92
ra <- 10

# informal group

ni <- 108+113
ri <- 21+26

# calculate the failure rates

ra/na
ri/ni
```

* Here is our output:

```Rout
> # arrest group
> 
> na <- 92
> ra <- 10
> 
> # informal group
> 
> ni <- 108+113
> ri <- 21+26
> 
> # calculate the failure rates
> 
> ra/na
[1] 0.1086957
> ri/ni
[1] 0.2126697
> 
```

* This information can be presented in a 2x2 contingency table as follows:

```R
t <- c(rep("A",92),rep("I",221))
y <- c(rep("no",92-10),rep("yes",10),rep("no",221-47),rep("yes",47))
table(y,t)
```

* The output is:

```Rout
> t <- c(rep("A",92),rep("I",221))
> y <- c(rep("no",92-10),rep("yes",10),rep("no",221-47),rep("yes",47))
> table(y,t)
     t
y       A   I
  no   82 174
  yes  10  47
> 
```

* Next, let's consider some ways of interpreting this table. The most obvious way is to examine the 2 failure rates like we did above:

```R
# work with the raw numbers

pa <- 10/(82+10)
pa
pi <- 47/(174+47)
pi
```

* which yields:

```Rout
> # work with the raw numbers
> 
> pa <- 10/(82+10)
> pa
[1] 0.1086957
> pi <- 47/(174+47)
> pi
[1] 0.2126697
> 
```

* Another approach is to do arithmetic on the table:

```R
t <- c(rep("A",92),rep("I",221))
y <- c(rep("no",92-10),rep("yes",10),rep("no",221-47),rep("yes",47))
mt <- table(y,t)
mt

c11 <- mt[1,1]
c11
c12 <- mt[1,2]
c12
c21 <- mt[2,1]
c21
c22 <- mt[2,2]
c22

pa <- c21/(c11+c21)
pa
pi <- c22/(c12+c22)
pi
```

* Here is our output:

```Rout
> t <- c(rep("A",92),rep("I",221))
> y <- c(rep("no",92-10),rep("yes",10),rep("no",221-47),rep("yes",47))
> mt <- table(y,t)
> mt
     t
y       A   I
  no   82 174
  yes  10  47
> 
> c11 <- mt[1,1]
> c11
[1] 82
> c12 <- mt[1,2]
> c12
[1] 174
> c21 <- mt[2,1]
> c21
[1] 10
> c22 <- mt[2,2]
> c22
[1] 47
> 
> pa <- c21/(c11+c21)
> pa
[1] 0.1086957
> pi <- c22/(c12+c22)
> pi
[1] 0.2126697
> 
```

#### 12. Classical/Average Treatment Effect (C/ATE)

* Building on this, we can compute what is often called the *classical treatment effect* (CTE; a term used by Manski and Nagin (1998; Bounding disagreements about treatment effects: a case study of sentencing and recidivism. Sociological Methodology, 28:99-137) or the population *average treatment effect* (ATE; a more commonly used term) by:

```R
cte <- pi-pa
cte
```

* We can interpret both the sign and the magnitude of this difference:

```Rout
> cte <- pi-pa
> cte
[1] 0.103974
> 
```

* The sign means that the informal group had a higher failure rate than the arrested group. The 0.104 statistic means that the failure rate was 10.4 percentage points higher in the informal group (21.3%) than it was in the arrested group (10.9%). The ordering of the groups is arbitrary.

#### 13. Inference About the C/ATE

* It is insufficient to present a treatment effect estimate without a measure of uncertainty to accompany the estimate. 
* As a starting point, it is useful to think about a 90% confidence interval for the difference between the two proportions (failure rates).
* The normal approximation to the binomial is the standard textbook formula for the difference between two proportions:

```R
t <- c(rep("A",92),rep("I",221))
y <- c(rep("no",92-10),rep("yes",10),rep("no",221-47),rep("yes",47))
mt <- table(y,t)
mt

c11 <- mt[1,1]
c11
c12 <- mt[1,2]
c12
c21 <- mt[2,1]
c21
c22 <- mt[2,2]
c22

pa <- c21/(c11+c21)
pa
pi <- c22/(c12+c22)
pi

ta <- c11+c21
ta
ti <- c12+c22
ti

delta <- pi-pa
delta

se.delta.pt1 <- pi*(1-pi)/ti
se.delta.pt2 <- pa*(1-pa)/ta
se.delta <- sqrt(se.delta.pt1+se.delta.pt2)

mult <- qnorm(0.95)
mult

lcl.delta <- delta-mult*se.delta
lcl.delta
ucl.delta <- delta+mult*se.delta
ucl.delta
```

* Here is our output:

```Rout
> t <- c(rep("A",92),rep("I",221))
> y <- c(rep("no",92-10),rep("yes",10),rep("no",221-47),rep("yes",47))
> mt <- table(y,t)
> mt
     t
y       A   I
  no   82 174
  yes  10  47
> 
> c11 <- mt[1,1]
> c11
[1] 82
> c12 <- mt[1,2]
> c12
[1] 174
> c21 <- mt[2,1]
> c21
[1] 10
> c22 <- mt[2,2]
> c22
[1] 47
> 
> pa <- c21/(c11+c21)
> pa
[1] 0.1086957
> pi <- c22/(c12+c22)
> pi
[1] 0.2126697
> 
> ta <- c11+c21
> ta
[1] 92
> ti <- c12+c22
> ti
[1] 221
> 
> delta <- pi-pa
> delta
[1] 0.103974
> 
> se.delta.pt1 <- pi*(1-pi)/ti
> se.delta.pt2 <- pa*(1-pa)/ta
> se.delta <- sqrt(se.delta.pt1+se.delta.pt2)
> 
> mult <- qnorm(0.95)
> mult
[1] 1.644854
> 
> lcl.delta <- delta-mult*se.delta
> lcl.delta
[1] 0.03398157
> ucl.delta <- delta+mult*se.delta
> ucl.delta
[1] 0.1739665
> 
```

* Notice that this 90% confidence interval [0.034,0.174] does not include the number zero.
* Here is another way to approach the problem:

```R
y <- c(rep(0,92-10),rep(1,10),rep(0,221-47),rep(1,47))
t <- c(rep("A",92),rep("I",221))
table(y,t)
M <- glm(y~1+as.factor(t),family="binomial")
summary(M)
```

* Here is our output:

```Rout
> y <- c(rep(0,92-10),rep(1,10),rep(0,221-47),rep(1,47))
> t <- c(rep("A",92),rep("I",221))
> table(y,t)
   t
y     A   I
  0  82 174
  1  10  47
> M <- glm(y~1+as.factor(t),family="binomial")
> summary(M)

Call:
glm(formula = y ~ 1 + as.factor(t), family = "binomial")

Coefficients:
              Estimate Std. Error z value Pr(>|z|)    
(Intercept)    -2.1041     0.3349  -6.282 3.34e-10 ***
as.factor(t)I   0.7952     0.3731   2.131   0.0331 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 297.08  on 312  degrees of freedom
Residual deviance: 291.98  on 311  degrees of freedom
AIC: 295.98

Number of Fisher Scoring iterations: 4

> 
```

* Now, we can use this information to recover the estimated failure rates:

```R
t <- 0
pa <- exp(-2.1041+0.7952*t)/(1+exp(-2.1041+0.7952*t))
pa
t <- 1
pi <- exp(-2.1041+0.7952*t)/(1+exp(-2.1041+0.7952*t))
pi
delta <- pi-pa
delta
```

* Here are the results:

```R
> t <- 0
> pa <- exp(-2.1041+0.7952*t)/(1+exp(-2.1041+0.7952*t))
> pa
[1] 0.108699
> t <- 1
> pi <- exp(-2.1041+0.7952*t)/(1+exp(-2.1041+0.7952*t))
> pi
[1] 0.212671
> delta <- pi-pa
> delta
[1] 0.103972
>
```

* Next, we can use the bootstrap to build the confidence interval for delta:

```R
set.seed(387)
y <- c(rep(0,92-10),rep(1,10),rep(0,221-47),rep(1,47))
t <- c(rep("A",92),rep("I",221))
table(y,t)
M <- glm(y~1+as.factor(t),family="binomial")
summary(M)

deltab <- vector()

for(i in 1:3000){
  b <- sample(1:313,size=313,replace=T)
  yb <- y[b]
  tb <- t[b]
  Mb <- glm(yb~1+as.factor(tb),family="binomial")
  intb <- coef(Mb)[1]
  slopeb <- coef(Mb)[2]
  pa <- exp(intb)/(1+exp(intb))
  pi <- exp(intb+slopeb)/(1+exp(intb+slopeb))
  deltab[i] <- pi-pa
  }

quantile(deltab,0.05)
quantile(deltab,0.95)
```

* Here is our output:

```Rout
> set.seed(387)
> y <- c(rep(0,92-10),rep(1,10),rep(0,221-47),rep(1,47))
> t <- c(rep("A",92),rep("I",221))
> table(y,t)
   t
y     A   I
  0  82 174
  1  10  47
> M <- glm(y~1+as.factor(t),family="binomial")
> summary(M)

Call:
glm(formula = y ~ 1 + as.factor(t), family = "binomial")

Coefficients:
              Estimate Std. Error z value Pr(>|z|)    
(Intercept)    -2.1041     0.3349  -6.282 3.34e-10 ***
as.factor(t)I   0.7952     0.3731   2.131   0.0331 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 297.08  on 312  degrees of freedom
Residual deviance: 291.98  on 311  degrees of freedom
AIC: 295.98

Number of Fisher Scoring iterations: 4

> 
> deltab <- vector()
> 
> for(i in 1:3000){
+   b <- sample(1:313,size=313,replace=T)
+   yb <- y[b]
+   tb <- t[b]
+   Mb <- glm(yb~1+as.factor(tb),family="binomial")
+   intb <- coef(Mb)[1]
+   slopeb <- coef(Mb)[2]
+   pa <- exp(intb)/(1+exp(intb))
+   pi <- exp(intb+slopeb)/(1+exp(intb+slopeb))
+   deltab[i] <- pi-pa
+   }
> 
> quantile(deltab,0.05)
        5% 
0.03367814 
> quantile(deltab,0.95)
      95% 
0.1753778 
> 
```
* These results are very close to the 90% interval based on the normal approximation described above.

#### 14. Normal Approximation Coverage Rate for Delta

* For today, let's check on the coverage rates of the normal approximation:

```R
set.seed(503)
na <- 92
ni <- 221
pa <- 10/92
pa
pi <- 47/221
pi
pop.delta <- pi-pa
pop.delta
mult <- qnorm(0.95)
mult

delta <- vector()
se.delta <- vector()
lcl.normal <- vector()
ucl.normal <- vector()
lcl.boot <- vector()
ucl.boot <- vector()

for(i in 1:10000) {
  ra <- ifelse(runif(n=na,min=0,max=1)<pa,1,0)
  ri <- ifelse(runif(n=ni,min=0,max=1)<pi,1,0)
  phata <- mean(ra)
  phati <- mean(ri)
  delta[i] <- phati-phata
  var.phata <- phata*(1-phata)/na
  var.phati <- phati*(1-phati)/ni
  se.delta[i] <- sqrt(var.phata+var.phati)
  lcl.normal[i] <- delta[i]-mult*se.delta[i]
  ucl.normal[i] <- delta[i]+mult*se.delta[i] 
  }

mean(delta)
sd(delta)
mean(se.delta)
mean(lcl.normal)
mean(ucl.normal)
mean(ifelse(lcl.normal<pop.delta & ucl.normal>pop.delta,1,0))
```

* And, here is our output:

```Rout
> set.seed(503)
> na <- 92
> ni <- 221
> pa <- 10/92
> pa
[1] 0.1086957
> pi <- 47/221
> pi
[1] 0.2126697
> pop.delta <- pi-pa
> pop.delta
[1] 0.103974
> mult <- qnorm(0.95)
> mult
[1] 1.644854
> 
> delta <- vector()
> se.delta <- vector()
> lcl.normal <- vector()
> ucl.normal <- vector()
> lcl.boot <- vector()
> ucl.boot <- vector()
> 
> for(i in 1:10000) {
+   ra <- ifelse(runif(n=na,min=0,max=1)<pa,1,0)
+   ri <- ifelse(runif(n=ni,min=0,max=1)<pi,1,0)
+   phata <- mean(ra)
+   phati <- mean(ri)
+   delta[i] <- phati-phata
+   var.phata <- phata*(1-phata)/na
+   var.phati <- phati*(1-phati)/ni
+   se.delta[i] <- sqrt(var.phata+var.phati)
+   lcl.normal[i] <- delta[i]-mult*se.delta[i]
+   ucl.normal[i] <- delta[i]+mult*se.delta[i] 
+   }
> 
> mean(delta)
[1] 0.1043082
> sd(delta)
[1] 0.04284055
> mean(se.delta)
[1] 0.0422513
> mean(lcl.normal)
[1] 0.03481097
> mean(ucl.normal)
[1] 0.1738054
> mean(ifelse(lcl.normal<pop.delta & ucl.normal>pop.delta,1,0))
[1] 0.8925
> 
```
