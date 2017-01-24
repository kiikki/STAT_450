# The Effct of New Drug on Malaria Infected Cells
Jon Sunwoo Kim (37355112)  

### Summary ###

We are interested in comparing the effect of a new malarial drug to the one currently used in the market to see whether we have strong evidence of better performance. The cortical tension data was collected for cells treated by 4 different antimalarial drug types, including the new drug. We tested our hypotheses using multiple statistical tests including pairwise two sample t-test and ANOVA further backed by regression. From our tests, we conclude that there is strong evidence that the new drug performs better than the ones currently in the market, by increasing malaria infected cells' cortical tension by a greater amount.


### Introduction ###

Malaria is a disease spread by mosquito bites, in which nearly half the world's population is at risk of transmission. In a report published by the World Health Organization, the number of deaths from malaria was estimated to be roughly 429,000 in 2015. This number is a reduction of 29% compared to the estimate in 2010, thanks to the advancement in the malarial cure drugs.


Our clients from UC San Francisco are studying a new antimalarial drug, and requested us to compare the effects of the new drug (drug X) and the drug currently used in the market (Chloroquine). Malaria infected red blood cells (RBCs) have greater rigidity due to the presence of the parasite. If the cells get too rigid, they are not able to pass through the capillaries, and it becomes easier for the immune system to detect and destroy these infected cells. Therefore, a better performing drug will make the infected RBCs more rigid. This rigidity can be measured through the cortical tension of the RBCs. The experiment from our clients tests whether treatment of drug X significantly increases cortical tension of the infected RBCs compared to other drugs. The clients observed the number of infected RBCs decreasing at a faster rate in rat infected by malaria when treated with drug X compared to the incumbent drug Chloroquine and suspected drug X to significantly perform better than the one currently used in the market. They requested us to perform statistical analysis to test their hypothesis.


Data was collected using a tool consisting of microfluidic device with microscale funnel constrictions to measure the pressure required to squeeze and push cells through the funnels. The pressure required, size of the cell, and the geometry of the funnel are used to calculate the rigidity of the cells, expressed as cortical tension. The experiment was performed at least three times for each drug treatment, and the data was pooled together for comparison of the mean cortical tension of each treatment group.


This report lays out and discusses the various statistical methods used to test for significance in the treatment effect for drug X. In the following sections, the details of the analysis processes including the pairwise t-test, analysis of variance, and regression are addressed.


### Results ###

The data has a total of 446 observations coming from 7 different groups. The data first had to be transformed into long form, for us to analyze it using R. It is already known that the antimalarial drugs have a significant effect in increasing the cortical tension of RBCs, and since we are only interested in malaria infected cells, the cortical tension values for the control group with inactive malarial parasite were removed from the dataset as they were outside of our interest, reducing the number of observations to 243. So we are left with the following treatment groups in our final dataset:


* Drug X - the new antimalarial drug
* Drug Z - antimalarial drug produced by Novartis company
* Chloroquine - the antimalarial drug currently used in the market
* Mefloquine - another antimalarial similar to Chloroquine


It is always a good idea to visually represent the data to get an overview [plot 1]. The box plot of the data hints the presence of multiple outliers, and thus a right skewed distribution of the data. A quick Q-Q plot of the sample data's quantile plotted against the standard normal distribution's quantile will show a linear relationship if the sample data follows an approximately normal distribution. In our data, we can see that there is a non-linear relationship in our Q-Q plot [plot 2], and this can also be illustrated using a histogram [plot 3]. Therefore, transformation is required to justify our normality assumption. The required power transformation can be found by running a Box-Cox test for normality. Our Box-Cox test result shows a suggested lambda value of -0.5 [plot 4], and taking the inverse square root of the response variable (CT) will give us an approximately normally distributed set of data [plot 5 & 6]. However, this exponent of -0.5 makes interpretation of the result very hard, so we will transform the data using a log transforamtion, which sufficiently justifies our normality assumption as well [plot 7].


The easiest way of checking whether drug X is better performing than Chloroquine is to perform one-sided two sample t-test. The test can be interpretted using the following null hypothesis and the alternative hypothesis:
$$H_{0}: \mu_{X} = \mu_{Ch}$$
$$H_{A}: \mu_{X} > \mu_{Ch}$$
When the mean cortical tension value of drug X treated cells is tested against the mean cortical tension value of Chloroquine treated cells, we see that under the null hypothesis, the probability of observing a difference greater than or equal to our observed difference (the p-value) is close to zero. Thus, there is very strong evidence that the population mean cortical tension of drug X treated cells is higher than the population mean cortical tension of Chloroquine treated cells [test 1]. In a similar fashion, the one-sided two sample t-test can be applied to other drugs in our dataset. From the two sample t-tests, we can see that there is very strong evidence that the population mean cortical tension of drug X treated cells is higher than both drug Z and Mefloquine treated cells [test 2 & 3]. The result from simultaneous comparison, using the pairwise t-test, takes into account the p-value adjustment and could be more accurate. The result shows the same conclusion [test 4].


We can also use ANOVA to see whether the population means for each group are the same or not. We can test our following hypotheses using ANOVA:
$$H_{0}: \mu_{X} = \mu_{Z} = \mu_{Ch} = \mu_{M}$$
$$H_{A}: \mu_{X} \neq \mu_{Z} \neq \mu_{Ch} \neq \mu_{M}$$
From our test result [test 5] we can only conclude that there is strong evidence that at least one of the drugs have different treatment effect. We do not know which one is different, and by how much. For that reason, we run a regression model. The regression takes the form
$$\log(CT)_{i} = \beta_{0} + \beta_{1,i}x_{i} + \epsilon_{i}$$
$\beta_{0}: \mbox{intercept (sample mean of the baseline)} \\ \beta_{1,i}: \mbox{treatment coefficient for drug i} \\ x_{i}: \mbox{treatment dummy variable for drug i} \\ \epsilon_{i}: \mbox{error term for drug i}\\$


and the coefficient estimate, $\hat{\beta_{1,i}}$, represents the treatment effect for each drug on the mean of log cortical tension, relative to the baseline. Therefore, to get the treatment effect for each drug on the mean of cortical tension, we need to take the inverse of log, $\exp(\beta_{1,i})$. Our regression model [test 6] calculates the least squares coefficient estimate for the treatment effect of drug X to be $\hat{\beta_{1,X}}=0.24$, indicating that treatment of drug X causes the cell to have a mean cortical tension value of $\exp(0.24)=1.27$ greater than the baseline, Chloroquine. The value of $\hat{\beta_{1,Z}}$ is significant, but negative, and  $\hat{\beta_{1,M}}$ is not significantly different from 0. From this, we interpret that there is not enough evidence that either drug Z or Mefloquine performs better than Chloroquine. Combining all our findings from the regression model, we see evidence that drug X has the strongest performance out of the four antimalarial drugs.


Our clients showed concern about the time spent inside the incubator potentially affecting the effectiveness of drug X and provided us with data. We see a very low correlation between the total time spent inside the incubator and the cell's cortical tension [plot 8].  


### Conclusions ###

We found very strong evidence that drug X increases the cortial tension of infected cells by a greater amount compared to any other drug. All of our statistical tests support this conclusion. In fact, our regression model estimates the mean treatment effect from drug X on cortical tension to be 1.27 units greater than Chloroquine, which was found to be the second best after drug X. In regards to our client's concerns, we found the correlation to be very low between the total time spent inside the incubator and the cell's cortical tension from which we conclude there is no or very weak effect of time spent inside the incubator to the cell's cortical tension.


Future recommendation for our clients is to collect more data so that we would see more of an approximately normal distribution in our dataset. In our analysis, we had to go through a transformation to meet the normality assumption in our tests, but with more data approximately normal distribution can be easily met so that we do not have to transform it. Also, there is a great possibility of lurking variables that we have not caught, since our regression only depends on the drug used. If more variables that might affect the cell's cortical tension level are also observed, we may have a stronger model to test our hypotheses.


### Appendix ###

```r
library(dplyr)
library(reshape2)
library(MASS)
library(ggplot2)

# Load dataset ----------------------------------------------------------------
dat <- read.csv("Malaria_data.csv", header = TRUE)
head(dat)
```

```
##   Infected X.Inactive X.Active Z.Inactive Z.Active Chloroquine Mefloquine
## 1    18.30      19.31    22.29      19.29    16.51       28.22      14.23
## 2    30.30      17.81    25.66      30.95    17.42       11.07      20.55
## 3    23.48      14.84    13.77      22.82     9.84       24.27      25.16
## 4    17.52      15.02    21.06      14.72    28.00       25.55      22.34
## 5    19.53      13.63    30.79      12.45    11.70       21.87      15.91
## 6    21.46      17.68    41.01      11.61    20.35       28.90      11.99
```

```r
# Convert data into long form
malaria <- dat %>% 
  melt(variable.name="drug", value.name="CT") %>% 
  filter(!is.na(CT)) %>%  # take out NA
  filter(drug %in% 
           c("X.Active", "Z.Active", "Chloroquine", "Mefloquine"))  # filter variables of interest

attach(malaria)
str(malaria)
```

```
## 'data.frame':	243 obs. of  2 variables:
##  $ drug: Factor w/ 7 levels "Infected","X.Inactive",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ CT  : num  22.3 25.7 13.8 21.1 30.8 ...
```

```r
# Check out data visually, using a box plot
# Plot 1
ggplot(data=malaria, aes(x=drug, y=CT)) + 
  geom_boxplot() + 
  ggtitle("Plot 1: Box plot of cortical tension for each drug")
```

![](Malaria_Report_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

```r
# Diagnostics of the dataset --------------------------------------------------
# Plot 2
ggplot(data=malaria, aes(sample=CT)) + 
  stat_qq() + 
  ggtitle("Plot 2: Q-Q plot of cortical tension")  # Normality test
```

![](Malaria_Report_files/figure-html/unnamed-chunk-1-2.png)<!-- -->

```r
# Plot 3
ggplot(data=malaria, aes(x=CT)) + 
  geom_histogram() + 
  ggtitle("Plot 3: Histogram of the cortical tension data")
```

![](Malaria_Report_files/figure-html/unnamed-chunk-1-3.png)<!-- -->

```r
# Box-Cox Test to figure out transformation
# Plot 4
boxcox(CT ~ drug, ylab="Log likelihood")
```

![](Malaria_Report_files/figure-html/unnamed-chunk-1-4.png)<!-- -->

```r
# Transform
malaria <- mutate(malaria, trans.CT=CT^(-0.5), log.CT=log(CT))
detach(malaria)
attach(malaria)

# Plot 5
ggplot(data=malaria, aes(x=drug, y=trans.CT)) + 
  geom_boxplot() + 
  ggtitle("Plot 5: Box plot of inverse square root of cortical tension for each drug")
```

![](Malaria_Report_files/figure-html/unnamed-chunk-1-5.png)<!-- -->

```r
# Plot 6
ggplot(data=malaria, aes(sample=trans.CT)) + 
  stat_qq() + 
  ggtitle("Plot 6: Q-Q plot of inverse square root of cortical tension")
```

![](Malaria_Report_files/figure-html/unnamed-chunk-1-6.png)<!-- -->

```r
# Plot 7
ggplot(data=malaria, aes(sample=log.CT)) + 
  stat_qq() + 
  ggtitle("Plot 7: Q-Q plot of log of cortical tension")
```

![](Malaria_Report_files/figure-html/unnamed-chunk-1-7.png)<!-- -->

```r
# Analysis --------------------------------------------------------------------

X.Active <- filter(malaria, drug=="X.Active")
Z.Active <- filter(malaria, drug=="Z.Active")
Chloroquine <- filter(malaria, drug=="Chloroquine")
Mefloquine <- filter(malaria, drug=="Mefloquine")

# Two sample t test
# Test 1
t.test(X.Active$log.CT, Chloroquine$log.CT, alternative="greater", mu=0)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  X.Active$log.CT and Chloroquine$log.CT
## t = 3.94, df = 169.8, p-value = 5.942e-05
## alternative hypothesis: true difference in means is greater than 0
## 95 percent confidence interval:
##  0.1390756       Inf
## sample estimates:
## mean of x mean of y 
##  3.300507  3.060817
```

```r
# Test 2
t.test(X.Active$log.CT, Mefloquine$log.CT, alternative="greater", mu=0)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  X.Active$log.CT and Mefloquine$log.CT
## t = 4.8085, df = 37.844, p-value = 1.215e-05
## alternative hypothesis: true difference in means is greater than 0
## 95 percent confidence interval:
##  0.2445234       Inf
## sample estimates:
## mean of x mean of y 
##  3.300507  2.923939
```

```r
# Test 3
t.test(X.Active$log.CT, Z.Active$log.CT, alternative="greater", mu=0)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  X.Active$log.CT and Z.Active$log.CT
## t = 5.869, df = 119.17, p-value = 2.017e-08
## alternative hypothesis: true difference in means is greater than 0
## 95 percent confidence interval:
##  0.2734632       Inf
## sample estimates:
## mean of x mean of y 
##  3.300507  2.919396
```

```r
# Test 4
pairwise.t.test(log.CT, drug, pool.sd=FALSE)
```

```
## 
## 	Pairwise comparisons using t tests with non-pooled SD 
## 
## data:  log.CT and drug 
## 
##             X.Active Z.Active Chloroquine
## Z.Active    2.4e-07  -        -          
## Chloroquine 0.00048  0.09154  -          
## Mefloquine  0.00012  0.95569  0.17508    
## 
## P value adjustment method: holm
```

```r
# ANOVA
# Test 5
malaria.aov <- aov(log.CT ~ drug, data=malaria)
summary(malaria.aov)
```

```
##              Df Sum Sq Mean Sq F value   Pr(>F)    
## drug          3   6.01  2.0032   13.78 2.55e-08 ***
## Residuals   239  34.74  0.1453                     
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
# Linear Regression
# Test 6
malaria$drug <- relevel(malaria$drug, ref="Chloroquine")  # make Chloroquine the baseline for comparison
malaria.lm <- lm(log.CT ~ drug, data=malaria)
summary(malaria.lm)
```

```
## 
## Call:
## lm(formula = log.CT ~ drug, data = malaria)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.84349 -0.27709 -0.03858  0.20969  1.17626 
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)     3.06082    0.04210  72.702  < 2e-16 ***
## drugX.Active    0.23969    0.05820   4.118 5.26e-05 ***
## drugZ.Active   -0.14142    0.06799  -2.080   0.0386 *  
## drugMefloquine -0.13688    0.09508  -1.440   0.1513    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.3812 on 239 degrees of freedom
## Multiple R-squared:  0.1475,	Adjusted R-squared:  0.1368 
## F-statistic: 13.78 on 3 and 239 DF,  p-value: 2.546e-08
```

```r
# Time data -------------------------------------------------------------------
malaria.time <- read.csv("Malaria_dataTime.csv") %>%
  na.omit
qplot(x=X.Active, y=Time, data=malaria.time)
```

![](Malaria_Report_files/figure-html/unnamed-chunk-1-8.png)<!-- -->

```r
str(malaria.time)
```

```
## 'data.frame':	90 obs. of  2 variables:
##  $ X.Active: num  22.3 25.7 13.8 21.1 30.8 ...
##  $ Time    : num  149 161 179 226 230 237 247 251 255 261 ...
##  - attr(*, "na.action")=Class 'omit'  Named int [1:4] 91 92 93 94
##   .. ..- attr(*, "names")= chr [1:4] "91" "92" "93" "94"
```

```r
cor(malaria.time$X.Active, malaria.time$Time)
```

```
## [1] 0.08812086
```
