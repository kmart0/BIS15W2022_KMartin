---
title: "Lab 8 Homework"
author: "Kelsey Martin"
date: "2022-02-02"
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
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

(I have rejected 'here')

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <- readr::read_csv("data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at C:/Users/kemar/Documents/GitHub/BIS15W2022_KMartin
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv("data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
sydneybeaches <- janitor::clean_names(sydneybeaches)
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?
The data are in long format, and it is considered "tidy" for R because each variable has its own column.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`


```r
sydneybeaches_long <- sydneybeaches %>% 
  select(site, date, enterococci_cfu_100ml)
```


4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`


```r
sydneybeaches_wide <- sydneybeaches_long %>% pivot_wider(
  names_from = date,
  values_from = enterococci_cfu_100ml
) %>% print()
```

```
## # A tibble: 11 x 345
##    site         `02/01/2013` `06/01/2013` `12/01/2013` `18/01/2013` `30/01/2013`
##    <chr>               <dbl>        <dbl>        <dbl>        <dbl>        <dbl>
##  1 Clovelly Be~           19            3            2           13            8
##  2 Coogee Beach           15            4           17           18           22
##  3 Gordons Bay~           NA           NA           NA           NA           NA
##  4 Little Bay ~            9            3           72            1           44
##  5 Malabar Bea~            2            4          390           15           13
##  6 Maroubra Be~            1            1           20            2           11
##  7 South Marou~            1            0           33            2           13
##  8 South Marou~           12            2          110           13          100
##  9 Bondi Beach             3            1            2            1            6
## 10 Bronte Beach            4            2           38            3           25
## 11 Tamarama Be~            1            0            7           22           23
## # ... with 339 more variables: `05/02/2013` <dbl>, `11/02/2013` <dbl>,
## #   `23/02/2013` <dbl>, `07/03/2013` <dbl>, `25/03/2013` <dbl>,
## #   `02/04/2013` <dbl>, `12/04/2013` <dbl>, `18/04/2013` <dbl>,
## #   `24/04/2013` <dbl>, `01/05/2013` <dbl>, `20/05/2013` <dbl>,
## #   `31/05/2013` <dbl>, `06/06/2013` <dbl>, `12/06/2013` <dbl>,
## #   `24/06/2013` <dbl>, `06/07/2013` <dbl>, `18/07/2013` <dbl>,
## #   `24/07/2013` <dbl>, `08/08/2013` <dbl>, `22/08/2013` <dbl>, ...
```


5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_wide %>%
  pivot_longer(-(site),
    names_to = "date",
    values_to = "enterococci_cfu_100ml"
  )
```

```
## # A tibble: 3,784 x 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ... with 3,774 more rows
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.


```r
sydneybeaches_long_seperate <- sydneybeaches_long %>% 
  separate(date, into=c("month","day","year"), sep="/") %>% 
  print()
```

```
## # A tibble: 3,690 x 5
##    site           month day   year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # ... with 3,680 more rows
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.


```r
sydneybeaches_long_seperate %>% 
  group_by(year, site) %>% 
  summarise(avg_enterococci_cfu_100ml=mean(enterococci_cfu_100ml,na.rm=T))
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 66 x 3
## # Groups:   year [6]
##    year  site                    avg_enterococci_cfu_100ml
##    <chr> <chr>                                       <dbl>
##  1 2013  Bondi Beach                                 32.2 
##  2 2013  Bronte Beach                                26.8 
##  3 2013  Clovelly Beach                               9.28
##  4 2013  Coogee Beach                                39.7 
##  5 2013  Gordons Bay (East)                          24.8 
##  6 2013  Little Bay Beach                           122.  
##  7 2013  Malabar Beach                              101.  
##  8 2013  Maroubra Beach                              47.1 
##  9 2013  South Maroubra Beach                        39.3 
## 10 2013  South Maroubra Rockpool                     96.4 
## # ... with 56 more rows
```



8. Make the output from question 7 easier to read by pivoting it to wide format.


```r
grouped_sydneybeaches_l_s <- sydneybeaches_long_seperate %>% 
  group_by(year, site) %>% 
  summarise(avg_enterococci_cfu_100ml=mean(enterococci_cfu_100ml,na.rm=T))
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups`
## argument.
```

```r
grouped_sydneybeaches_l_s %>% 
  pivot_wider(
    names_from = year,
    values_from = avg_enterococci_cfu_100ml
  )
```

```
## # A tibble: 11 x 7
##    site                    `2013` `2014` `2015` `2016` `2017` `2018`
##    <chr>                    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Bondi Beach              32.2   11.1   14.3    19.4  13.2   22.9 
##  2 Bronte Beach             26.8   17.5   23.6    61.3  16.8   43.4 
##  3 Clovelly Beach            9.28  13.8    8.82   11.3   7.93  10.6 
##  4 Coogee Beach             39.7   52.6   40.3    59.5  20.7   21.6 
##  5 Gordons Bay (East)       24.8   16.7   36.2    39.0  13.7   17.6 
##  6 Little Bay Beach        122.    19.5   25.5    31.2  18.2   59.1 
##  7 Malabar Beach           101.    54.5   66.9    91.0  49.8   38.0 
##  8 Maroubra Beach           47.1    9.23  14.5    26.6  11.6    9.21
##  9 South Maroubra Beach     39.3   14.9    8.25   10.7   8.26  12.5 
## 10 South Maroubra Rockpool  96.4   40.6   47.3    59.3  46.9  112.  
## 11 Tamarama Beach           29.7   39.6   57.0    50.3  20.4   15.5
```

9. What was the most polluted beach in 2018?


```r
grouped_sydneybeaches_l_s %>% 
  filter(year==2018) %>% 
  arrange(desc(avg_enterococci_cfu_100ml)) %>% head(n=1)
```

```
## # A tibble: 1 x 3
## # Groups:   year [1]
##   year  site                    avg_enterococci_cfu_100ml
##   <chr> <chr>                                       <dbl>
## 1 2018  South Maroubra Rockpool                      112.
```



10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
