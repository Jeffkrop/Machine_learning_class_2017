Homework One
================
2018-02-11

R code for homework 1

``` r
#Read in data for homework 1 and name it Iris
Iris <- read.csv("FisherIris_MDL.csv", header = FALSE)
```

``` r
#Get the dimension of the dataset. 
dim(Iris)
```

    ## [1] 150   5

There are 150 rows and 5 columns

``` r
#See what rows have a 0 in the 5th column 
which(Iris$V5 < 0)
```

    ## [1]  11  24  59  90 109 137

Rows 11, 24, 59, 90, 109, 137 have zeros in column 5

``` r
#Get only rows that are greater than zero
Iris <- Iris %>% filter(V5 >= 0)

#Get the dimension of the new dataset with zeros removed.
dim(Iris)
```

    ## [1] 144   5

After filtering Iris to remove zeros from the 5th column there are now 144 rows and 5 columns

``` r
#Makes columns 1-4 new dataframe called X
X <- Iris[,1:4]

#Makes columns 5 new dataframe called Y
Y <- Iris[,5]
```

``` r
#Find minimum value for each column in X
apply(X, 2, min)
```

    ## V1 V2 V3 V4 
    ##  1 10 20 43

``` r
#Find max of each column in X
apply(X,2,max)
```

    ## V1 V2 V3 V4 
    ## 25 69 44 79
