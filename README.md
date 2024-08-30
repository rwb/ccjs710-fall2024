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

mult.lower <- qnorm(p=0.025)
mult.lower

mult.upper <- qnorm(p=0.975)
mult.upper

for(i in 1:nsamples)
  {
   rs <- sample(r,size=sampsize,replace=T)
   rs_est[i] <- mean(rs)
   rs_std[i] <- sqrt(mean(rs)*(1-mean(rs))/sampsize)
   rs_lcl[i] <- rs_est[i]+mult.lower*rs_std[i]
   rs_ucl[i] <- rs_est[i]+mult.upper*rs_std[i]
   }

# look at the results

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
> # look at the results
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
