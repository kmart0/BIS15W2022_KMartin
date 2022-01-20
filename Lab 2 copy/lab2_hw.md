---
title: "Lab 2 Homework"
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

1. What is a vector in R?  

A vector is a series of data. The data can be words, numbers, etc but it is basically how R recognizes multiple numbers seperately instead of trying to add them all together as an arithmatic function

2. What is a data matrix in R?  

A data matrix is similar to a data table and is basically vectors/data arrangements stacked on top of each other

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  

```r
#need to make a new object that puts all the hot springs into its own vector
hot_spring_data <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
hot_spring_data
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

```r
#now I can make this into a table
hot_spring_data_table <- matrix(hot_spring_data, nrow=8, byrow=T)
hot_spring_data_table
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```




5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.


```r
#making my vectors for the row and column titles
hot_spring_names <- c("Bluebell Spring","Opal Spring","Riverside Spring","Too Hot Spring","Mystery Spring", "Emerald Spring","Black Spring","Pearl Spring")
temps_of_springs <- c("Jill", "Steve", "Susan")
```


```r
#Adding my names to the matrix
colnames(hot_spring_data_table) <- temps_of_springs
rownames(hot_spring_data_table) <- hot_spring_names
hot_spring_data_table
```

```
##                   Jill Steve Susan
## Bluebell Spring  36.25 35.40 35.30
## Opal Spring      35.15 35.35 33.35
## Riverside Spring 30.70 29.65 29.20
## Too Hot Spring   39.70 40.05 38.65
## Mystery Spring   31.85 31.40 29.30
## Emerald Spring   30.20 30.65 29.75
## Black Spring     32.90 32.50 32.80
## Pearl Spring     36.80 36.45 33.15
```


6. Calculate the mean temperature of all eight springs.


```r
#make a new vector that calculates the means of the rows
Average_Temp <- rowMeans(hot_spring_data_table)
Average_Temp
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##         35.65000         34.61667         29.85000         39.46667 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##         30.85000         30.20000         32.73333         35.46667
```


7. Add this as a new column in the data matrix.  

```r
#Add the new vector to the matrix, make new table
complete_spring_temp_table <- cbind(hot_spring_data_table,Average_Temp)
complete_spring_temp_table
```

```
##                   Jill Steve Susan Average_Temp
## Bluebell Spring  36.25 35.40 35.30     35.65000
## Opal Spring      35.15 35.35 33.35     34.61667
## Riverside Spring 30.70 29.65 29.20     29.85000
## Too Hot Spring   39.70 40.05 38.65     39.46667
## Mystery Spring   31.85 31.40 29.30     30.85000
## Emerald Spring   30.20 30.65 29.75     30.20000
## Black Spring     32.90 32.50 32.80     32.73333
## Pearl Spring     36.80 36.45 33.15     35.46667
```

8. Show Susan's value for Opal Spring only.

```r
complete_spring_temp_table[2,3]
```

```
## [1] 33.35
```

9. Calculate the mean for Jill's column only.  

```r
#Make Jill's column its own matrix
Jills_Measurements <- complete_spring_temp_table[,1]
Jills_measurements_table <- matrix(Jills_Measurements, nrow=8, byrow=T)
#sum the column and extract the value
Jills_measurements_mean <- colMeans(Jills_measurements_table)
Jills_measurements_mean
```

```
## [1] 34.19375
```



10. Use the data matrix to perform one calculation or operation of your interest.

```r
#I'm interested in standard deviations of measurements by the different scientists for each spring
#There's probably a much easier way to do this but since I don't know the function I am just going to find the sd of each row and make that a new vector to add to my table
Sd_Bluebell <- sd(hot_spring_data_table[1,])
Sd_Bluebell
```

```
## [1] 0.5220153
```

```r
#I'm going to check to make sure this is the value I want
values <- c(36.25,35.4,35.3)
sd(values)
```

```
## [1] 0.5220153
```

```r
#ok it worked! I will do the rest of them now
sd_Opal <- sd(hot_spring_data_table[2,])
sd_Riverside <- sd(hot_spring_data_table[3,])
sd_TooHot <- sd(hot_spring_data_table[4,])
sd_Mystery <- sd(hot_spring_data_table[5,])
sd_Emerald <- sd(hot_spring_data_table[6,])
sd_Black <- sd(hot_spring_data_table[7,])
sd_Pearl <- sd(hot_spring_data_table[8,])
#Now I will make a vector out of them
sd_springtemps <- c(Sd_Bluebell,sd_Opal,sd_Riverside,sd_TooHot,sd_Mystery,sd_Emerald,sd_Black,sd_Pearl)
#and then add the vector as a column
xtra_complete_spring_temp_table <- cbind(complete_spring_temp_table,sd_springtemps)
xtra_complete_spring_temp_table
```

```
##                   Jill Steve Susan Average_Temp sd_springtemps
## Bluebell Spring  36.25 35.40 35.30     35.65000      0.5220153
## Opal Spring      35.15 35.35 33.35     34.61667      1.1015141
## Riverside Spring 30.70 29.65 29.20     29.85000      0.7697402
## Too Hot Spring   39.70 40.05 38.65     39.46667      0.7285831
## Mystery Spring   31.85 31.40 29.30     30.85000      1.3610658
## Emerald Spring   30.20 30.65 29.75     30.20000      0.4500000
## Black Spring     32.90 32.50 32.80     32.73333      0.2081666
## Pearl Spring     36.80 36.45 33.15     35.46667      2.0139100
```
And somehow it worked!

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
