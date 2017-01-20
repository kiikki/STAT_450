# My Landing Page
Jon  

### Nice Intro About Myself ###

Hi, my name is Jon Kim and I am a fourth year student pursuing Statistics and Economics Double Majors at UBC.
My interest is in *Econometrical Analysis*, and I am a huge soccer fan.

![](next-gen-nike-mercurial-superfly-v-2.jpg)

I hope to have an awesome time during STAT450. I'm really looking forward to it.

---

### Malaria Analysis ###



```r
library(dplyr)
library(reshape2)
library(MASS)
library(ggplot2)
```


```r
setwd("C:/Users/MedPrime/Desktop/sunwoo/School/Stat 450/")

# Load dataset
dat <- read.csv("Malaria_data.csv", header = TRUE)

# Convert data into long form
malaria <- dat %>% 
  melt(variable.name="drug", value.name="CT") %>% 
  filter(!is.na(CT)) %>%  # take out NA
  filter(drug %in% c("X.Active", "Z.Active", "Chloroquine", "Mefloquine"))  # filter out variables of interest
```

```
## No id variables; using all as measure variables
```

```r
attach(malaria)

# Structure of the data
str(malaria)
```

```
## 'data.frame':	243 obs. of  2 variables:
##  $ drug: Factor w/ 7 levels "Infected","X.Inactive",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ CT  : num  22.3 25.7 13.8 21.1 30.8 ...
```

```r
# Check out data visually, using a box plot
ggplot(data=malaria, aes(x=drug, y=CT)) + 
  geom_boxplot() + 
  ggtitle("Box plot of cortical tension for each drug")
```

![](JonKim_Intro_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
# Diagnostics of the dataset
ggplot(data=malaria, aes(sample=CT)) + 
  stat_qq() + 
  ggtitle("Q-Q plot of cortical tension")  # Normality test
```

![](JonKim_Intro_files/figure-html/unnamed-chunk-2-2.png)<!-- -->

We can see the data does not comply with our normally distributed assumption. Therefore, we need to transform the data.
