---
title: "Lab 3 Homework"
author: "Kelsey Martin"
date: "2022-01-20"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
#help.search("sleep")
#mammal sleep dataset taken from the Proceedings of the National Academy of Sciences
```

2. Store these data into a new data frame `sleep`.

```r
sleep <- msleep
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

```r
dim(sleep)
```

```
## [1] 83 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
anyNA(sleep)
```

```
## [1] TRUE
```

5. Show a list of the column names is this data frame.

```r
colnames(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

```r
table(sleep$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```

There are 32 herbivores represented.

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
sm_mammals <- subset(sleep, bodywt<=1)
lg_mammals <- subset(sleep, bodywt>=200)
sm_mammals
```

```
## # A tibble: 36 x 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Owl m~ Aotus omni  Prim~ <NA>                17         1.8      NA       7  
##  2 Great~ Blar~ omni  Sori~ lc                  14.9       2.3       0.133   9.1
##  3 Vespe~ Calo~ <NA>  Rode~ <NA>                 7        NA        NA      17  
##  4 Guine~ Cavis herbi Rode~ domesticated         9.4       0.8       0.217  14.6
##  5 Chinc~ Chin~ herbi Rode~ domesticated        12.5       1.5       0.117  11.5
##  6 Star-~ Cond~ omni  Sori~ lc                  10.3       2.2      NA      13.7
##  7 Afric~ Cric~ omni  Rode~ <NA>                 8.3       2        NA      15.7
##  8 Lesse~ Cryp~ omni  Sori~ lc                   9.1       1.4       0.15   14.9
##  9 Big b~ Epte~ inse~ Chir~ lc                  19.7       3.9       0.117   4.3
## 10 Europ~ Erin~ omni  Erin~ lc                  10.1       3.5       0.283  13.9
## # ... with 26 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

```r
lg_mammals
```

```
## # A tibble: 7 x 11
##   name   genus  vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>  <chr>  <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Cow    Bos    herbi Arti~ domesticated         4         0.7       0.667  20  
## 2 Asian~ Eleph~ herbi Prob~ en                   3.9      NA        NA      20.1
## 3 Horse  Equus  herbi Peri~ domesticated         2.9       0.6       1      21.1
## 4 Giraf~ Giraf~ herbi Arti~ cd                   1.9       0.4      NA      22.1
## 5 Pilot~ Globi~ carni Ceta~ cd                   2.7       0.1      NA      21.4
## 6 Afric~ Loxod~ herbi Prob~ vu                   3.3      NA        NA      20.7
## 7 Brazi~ Tapir~ herbi Peri~ vu                   4.4       1         0.9    19.6
## # ... with 2 more variables: brainwt <dbl>, bodywt <dbl>
```

8. What is the mean weight for both the small and large mammals?

```r
mean(sm_mammals$bodywt)
```

```
## [1] 0.2596667
```


```r
mean(lg_mammals$bodywt)
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

```r
mean(sm_mammals$sleep_total)
```

```
## [1] 12.65833
```


```r
mean(lg_mammals$sleep_total)
```

```
## [1] 3.3
```

10. Which animal is the sleepiest among the entire dataframe?

(I could also view the table and sort by sleep_total value to see that the Little Brown Bat sleeps the most)


```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```


```r
sleepiest_mammal<- subset (sleep, sleep_total>=19.9)
sleepiest_mammal
```

```
## # A tibble: 1 x 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Little~ Myot~ inse~ Chir~ <NA>                19.9         2         0.2   4.1
## # ... with 2 more variables: brainwt <dbl>, bodywt <dbl>
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
