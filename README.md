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

*Instructions*: Please complete each of the tasks outlined below. When you are finished, please email a readable pdf copy of your work where your responses to each of the tasks and questions are clearly indicated. You are also welcome to submit a paper copy if you prefer (but any submissions arriving after 5pm on Friday 9/13 should be sent via email so there is a time record of the submission). Also, if you choose to submit a paper copy and I am not in the office when you submit, you should put your paper copy in an interoffice mail envelope and slide it under my office door. Please show all calculations in an organized and easy to read format. This is to help me grade your work but it is also so you can go back to your notes at a later date and be able to understand what you did and why you did it. A couple of points to keep in mind about late submissions: (1) if your assignment is submitted late (after 11:59pm on Friday 9/13/24), there will be an automatic 10 point deduction for a late submission; (2) if your assignment is submitted more than 24 hours late, there will be an automatic 20 point deduction; (3) if your assignment is submitted more than 48 hours late, the maximum available score on the assignment will be a 50. Please double check that I can read your pdf file before you submit it. If the file is corrupt or has a virus or I am unable to read it, it will be treated the same as a late submission. Once I receive your submission (which may not be immediately after you send it), I will write to you and confirm that I have received it and can open your file. The work you submit for this project must be your own work. I will also confirm receipt of paper submissions. If you choose to ask me a clarifying question, I will determine whether I think it is proper to answer the question. If I decide to answer the question, I will post both the question and my response below the assignment for all students to see. I will not be able to answer any clarifying questions after class on Thursday 9/12. I suggest you not submit your assignment until after that time. The point values for each task are listed next to the task. 

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

3. Suppose we have a state population of 7822 people released on bail. The population failure to appear (FTA) rate is 27% (which would ordinarily not be known to us). Suppose we are able to draw a single random sample of size N = 87 from this population and that 33 of the 87 people in our sample failed to appear. Estimate a 90% confidence interval for the FTA rate in the single sample (20pts). Comment on whether the 90% confidence interval in your sample includes the true population parameter value (10pts). 
