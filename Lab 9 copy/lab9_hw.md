---
title: "Lab 9 Homework"
author: "Kelsey Martin"
date: "2022-02-09"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
library(ggplot2)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- readr::read_csv("data/ca_college_data.csv")
```

```
## Rows: 341 Columns: 10
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.

```r
colleges <- janitor::clean_names(colleges)
names(colleges)
```

```
##  [1] "instnm"        "city"          "stabbr"        "zip"          
##  [5] "adm_rate"      "sat_avg"       "pcip26"        "costt4_a"     
##  [9] "c150_4_pooled" "pftftug1_ef"
```


```r
summary(colleges)
```

```
##     instnm              city              stabbr              zip           
##  Length:341         Length:341         Length:341         Length:341        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     adm_rate         sat_avg         pcip26           costt4_a    
##  Min.   :0.0807   Min.   : 870   Min.   :0.00000   Min.   : 7956  
##  1st Qu.:0.4581   1st Qu.: 985   1st Qu.:0.00000   1st Qu.:12578  
##  Median :0.6370   Median :1078   Median :0.00000   Median :16591  
##  Mean   :0.5901   Mean   :1112   Mean   :0.01981   Mean   :26685  
##  3rd Qu.:0.7461   3rd Qu.:1237   3rd Qu.:0.02457   3rd Qu.:39289  
##  Max.   :1.0000   Max.   :1555   Max.   :0.21650   Max.   :69355  
##  NA's   :240      NA's   :276    NA's   :35        NA's   :124    
##  c150_4_pooled     pftftug1_ef    
##  Min.   :0.0625   Min.   :0.0064  
##  1st Qu.:0.4265   1st Qu.:0.3212  
##  Median :0.5845   Median :0.5016  
##  Mean   :0.5705   Mean   :0.5577  
##  3rd Qu.:0.7162   3rd Qu.:0.8117  
##  Max.   :0.9569   Max.   :1.0000  
##  NA's   :221      NA's   :53
```


```r
naniar::miss_var_summary(colleges)
```

```
## # A tibble: 10 x 3
##    variable      n_miss pct_miss
##    <chr>          <int>    <dbl>
##  1 sat_avg          276     80.9
##  2 adm_rate         240     70.4
##  3 c150_4_pooled    221     64.8
##  4 costt4_a         124     36.4
##  5 pftftug1_ef       53     15.5
##  6 pcip26            35     10.3
##  7 instnm             0      0  
##  8 city               0      0  
##  9 stabbr             0      0  
## 10 zip                0      0
```

2. Which cities in California have the highest number of colleges?

```r
colleges %>% 
  group_by(city) %>% 
  count() %>% arrange(desc(n))
```

```
## # A tibble: 161 x 2
## # Groups:   city [161]
##    city              n
##    <chr>         <int>
##  1 Los Angeles      24
##  2 San Diego        18
##  3 San Francisco    15
##  4 Sacramento       10
##  5 Berkeley          9
##  6 Oakland           9
##  7 Claremont         7
##  8 Pasadena          6
##  9 Fresno            5
## 10 Irvine            5
## # ... with 151 more rows
```

3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.


```r
topten <- colleges %>% 
  group_by(city) %>% 
  count() %>% arrange(desc(n)) %>% head(n=10)
```



```r
topten %>% 
  group_by(city) %>%
    ggplot(aes(x=city, y=n)) + geom_col()+coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?

```r
colleges %>% 
  group_by(city) %>% 
  summarise(avg_cost_city=mean(costt4_a, na.rm=T)) %>% 
  arrange(desc(avg_cost_city))
```

```
## # A tibble: 161 x 2
##    city                avg_cost_city
##    <chr>                       <dbl>
##  1 Claremont                   66498
##  2 Malibu                      66152
##  3 Valencia                    64686
##  4 Orange                      64501
##  5 Redlands                    61542
##  6 Moraga                      61095
##  7 Atherton                    56035
##  8 Thousand Oaks               54373
##  9 Rancho Palos Verdes         50758
## 10 La Verne                    50603
## # ... with 151 more rows
```

5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).

```r
costly_college <- colleges %>% 
  filter(city=="Claremont"|city=="Davis") %>% 
  select(instnm, city, costt4_a)
```


```r
costly_college %>% 
  ggplot(aes(x=instnm, y=costt4_a))+geom_col()+coord_flip()
```

```
## Warning: Removed 2 rows containing missing values (position_stack).
```

![](lab9_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?


```r
names(colleges)
```

```
##  [1] "instnm"        "city"          "stabbr"        "zip"          
##  [5] "adm_rate"      "sat_avg"       "pcip26"        "costt4_a"     
##  [9] "c150_4_pooled" "pftftug1_ef"
```



```r
ggplot(data=colleges, mapping=aes(x=adm_rate, y=costt4_a))+geom_point()
```

```
## Warning: Removed 249 rows containing missing values (geom_point).
```

![](lab9_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

There isn't really a strong correlation in any direction in this graph- I'm going to interpret this as the cost of college and the relation to admission rate is fairly nonsensical and arbitrary (like many things about our country's education system)


7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?

```r
ggplot(data=colleges, mapping=aes(x=costt4_a, y=c150_4_pooled))+geom_point()
```

```
## Warning: Removed 225 rows containing missing values (geom_point).
```

![](lab9_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
Theres a pretty strong positive correlation between cost and completion rate, so I guess when people pay a lot for their college education they are more likely to complete their degree. (or the transfer students that do their GE's at cheaper schools affect this data) 


8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.

```r
theucs <- colleges %>% 
  filter(grepl("University of California", instnm))
```

Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
univ_ca_final <- theucs %>% 
  filter(!grepl("Hastings|San Francisco", instnm))
```

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
univ_ca_final_sep <- univ_ca_final %>% separate(instnm, c("univ", "campus"), sep="-")
```

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.

```r
univ_ca_final_sep %>%
  ggplot(aes(x=campus, y=adm_rate))+geom_col()+coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->


```r
univ_ca_final_sep %>% 
  count(campus, adm_rate)
```

```
## # A tibble: 8 x 3
##   campus        adm_rate     n
##   <chr>            <dbl> <int>
## 1 Berkeley         0.169     1
## 2 Davis            0.423     1
## 3 Irvine           0.406     1
## 4 Los Angeles      0.180     1
## 5 Riverside        0.663     1
## 6 San Diego        0.357     1
## 7 Santa Barbara    0.358     1
## 8 Santa Cruz       0.578     1
```

10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.

```r
names(univ_ca_final_sep)
```

```
##  [1] "univ"          "campus"        "city"          "stabbr"       
##  [5] "zip"           "adm_rate"      "sat_avg"       "pcip26"       
##  [9] "costt4_a"      "c150_4_pooled" "pftftug1_ef"
```


```r
univ_ca_final_sep %>% 
  ggplot(aes(x=reorder(campus,pcip26), y=pcip26))+geom_col()+coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-21-1.png)<!-- -->


```r
univ_ca_final_sep %>% 
  count(campus, pcip26)
```

```
## # A tibble: 8 x 3
##   campus        pcip26     n
##   <chr>          <dbl> <int>
## 1 Berkeley       0.105     1
## 2 Davis          0.198     1
## 3 Irvine         0.107     1
## 4 Los Angeles    0.155     1
## 5 Riverside      0.149     1
## 6 San Diego      0.216     1
## 7 Santa Barbara  0.108     1
## 8 Santa Cruz     0.193     1
```

UC San Diego awards the highest percentage of degrees in biological or biomedical sciences. (UC Davis is a pretty close second!

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
