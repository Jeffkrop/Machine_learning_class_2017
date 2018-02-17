Homework Two
================
2018-02-17

``` r
patients <- read.csv("patients.csv")
pairs(Systolic~Age + Weight + Height + Gender + Location + Smoker + SelfAssessedHealthStatus, data = patients)
```

![](homework_2_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-2-1.png)

#### Now I want to check that Gender, Smoker, Location, SelfAssessedHealthStatus are categorical with ones and zeros. To do this in R I need to see that they are factors.

``` r
str(patients) 
```

    ## 'data.frame':    100 obs. of  10 variables:
    ##  $ Age                     : int  38 43 38 40 49 46 33 40 28 31 ...
    ##  $ Diastolic               : int  93 77 83 75 80 70 88 82 78 86 ...
    ##  $ Gender                  : Factor w/ 2 levels "'Female'","'Male'": 2 2 1 1 1 1 1 2 2 1 ...
    ##  $ Height                  : int  71 69 64 67 64 68 64 68 68 66 ...
    ##  $ LastName                : Factor w/ 100 levels "'Adams'","'Alexander'",..: 84 45 96 46 11 22 55 97 57 86 ...
    ##  $ Location                : Factor w/ 3 levels "'County General Hospital'",..: 1 3 2 3 1 2 3 3 2 1 ...
    ##  $ SelfAssessedHealthStatus: Factor w/ 4 levels "'Excellent'",..: 1 2 3 2 3 3 3 3 1 1 ...
    ##  $ Smoker                  : int  1 0 0 0 0 0 1 0 0 0 ...
    ##  $ Systolic                : int  124 109 125 117 122 121 130 115 115 118 ...
    ##  $ Weight                  : int  176 163 131 133 119 142 142 180 183 132 ...

#### I can see that Gender, Location, SelfAssessedHealthStatus are factors I just need to change Smoker.

``` r
patients$Smoker <- as.factor(patients$Smoker)
str(patients)
```

    ## 'data.frame':    100 obs. of  10 variables:
    ##  $ Age                     : int  38 43 38 40 49 46 33 40 28 31 ...
    ##  $ Diastolic               : int  93 77 83 75 80 70 88 82 78 86 ...
    ##  $ Gender                  : Factor w/ 2 levels "'Female'","'Male'": 2 2 1 1 1 1 1 2 2 1 ...
    ##  $ Height                  : int  71 69 64 67 64 68 64 68 68 66 ...
    ##  $ LastName                : Factor w/ 100 levels "'Adams'","'Alexander'",..: 84 45 96 46 11 22 55 97 57 86 ...
    ##  $ Location                : Factor w/ 3 levels "'County General Hospital'",..: 1 3 2 3 1 2 3 3 2 1 ...
    ##  $ SelfAssessedHealthStatus: Factor w/ 4 levels "'Excellent'",..: 1 2 3 2 3 3 3 3 1 1 ...
    ##  $ Smoker                  : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 2 1 1 1 ...
    ##  $ Systolic                : int  124 109 125 117 122 121 130 115 115 118 ...
    ##  $ Weight                  : int  176 163 131 133 119 142 142 180 183 132 ...

#### Run the model with out normalizing the data.

``` r
t1 <- lm(Systolic ~ Age + Weight + Height + Gender + Location + Smoker + SelfAssessedHealthStatus, data = patients)

summary(t1)
```

    ## 
    ## Call:
    ## lm(formula = Systolic ~ Age + Weight + Height + Gender + Location + 
    ##     Smoker + SelfAssessedHealthStatus, data = patients)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -8.6277 -3.1293 -0.6898  3.1426 11.8390 
    ## 
    ## Coefficients:
    ##                                     Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)                         88.65811   18.22461   4.865 4.92e-06
    ## Age                                  0.08026    0.06700   1.198   0.2341
    ## Weight                              -0.01342    0.05837  -0.230   0.8187
    ## Height                               0.46962    0.25391   1.850   0.0677
    ## Gender'Male'                        -1.47939    3.26575  -0.453   0.6516
    ## Location'St. Mary's Medical Center' -0.85650    1.29799  -0.660   0.5110
    ## Location'VA Hospital'               -1.73484    1.13323  -1.531   0.1293
    ## Smoker1                              9.67309    1.04590   9.249 1.15e-14
    ## SelfAssessedHealthStatus'Fair'      -2.75097    1.51063  -1.821   0.0720
    ## SelfAssessedHealthStatus'Good'       0.58638    1.17833   0.498   0.6200
    ## SelfAssessedHealthStatus'Poor'       0.45934    1.67619   0.274   0.7847
    ##                                        
    ## (Intercept)                         ***
    ## Age                                    
    ## Weight                                 
    ## Height                              .  
    ## Gender'Male'                           
    ## Location'St. Mary's Medical Center'    
    ## Location'VA Hospital'                  
    ## Smoker1                             ***
    ## SelfAssessedHealthStatus'Fair'      .  
    ## SelfAssessedHealthStatus'Good'         
    ## SelfAssessedHealthStatus'Poor'         
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 4.713 on 89 degrees of freedom
    ## Multiple R-squared:  0.5569, Adjusted R-squared:  0.5071 
    ## F-statistic: 11.19 on 10 and 89 DF,  p-value: 3.894e-12

The regression coefficients are
Age 0.0802597
weight -0.0134183
height 0.4696206
gender Male weight -1.4793907
Location'St. Mary's Medical Center' -0.8565008
Location'VA Hospital' -1.7348405
Smoker 9.6730871
SelfAssessedHealthStatus'Fair' -2.7509682
SelfAssessedHealthStatus'Good' 0.5863787
SelfAssessedHealthStatus'Poor' 0.4593428

Looking at the summary of the regression model before it has been normalized there are some interesting this happening, if you are a male your blood pressure will be -1.47 as your age goes up 1 year your blood pressure will go up 0.08 as your weight goes up 1 your blood pressure will go down 0.01. It is interesting that is your self assessed health status is fair it goes down 2.75 but if it is good it goes up 0.58. It is clear that some work needs to be done with this data.

#### I want to see some plots of this data before is is normalized

``` r
par(mfrow=c(2,2))
plot(t1, which=1:4)
```

![](homework_2_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-6-1.png)

#### Set the column means to 0 and the standard deveation to 1 for Age, Height and Weight.

``` r
norm  <- patients %>% select(Age, Height, Weight)
norm <- as.data.frame(scale(norm))
```

The column means are 0, 0, 0 and the column standard deviations are 1, 1, 1

#### Add the normilized Age, Height, Weight columns back

``` r
norm_patients <- patients[,-c(1,4,10)]
norm_patients <- cbind(norm_patients, norm)
str(norm_patients)
```

    ## 'data.frame':    100 obs. of  10 variables:
    ##  $ Diastolic               : int  93 77 83 75 80 70 88 82 78 86 ...
    ##  $ Gender                  : Factor w/ 2 levels "'Female'","'Male'": 2 2 1 1 1 1 1 2 2 1 ...
    ##  $ LastName                : Factor w/ 100 levels "'Adams'","'Alexander'",..: 84 45 96 46 11 22 55 97 57 86 ...
    ##  $ Location                : Factor w/ 3 levels "'County General Hospital'",..: 1 3 2 3 1 2 3 3 2 1 ...
    ##  $ SelfAssessedHealthStatus: Factor w/ 4 levels "'Excellent'",..: 1 2 3 2 3 3 3 3 1 1 ...
    ##  $ Smoker                  : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 2 1 1 1 ...
    ##  $ Systolic                : int  124 109 125 117 122 121 130 115 115 118 ...
    ##  $ Age                     : num  -0.0388 0.6542 -0.0388 0.2384 1.4857 ...
    ##  $ Height                  : num  1.3855 0.6804 -1.0823 -0.0247 -1.0823 ...
    ##  $ Weight                  : num  0.828 0.339 -0.866 -0.79 -1.317 ...

### **Question 3**

#### Run a new model.

``` r
t2 <- lm(Systolic~Age + Weight + Height + Gender + Location + Smoker + SelfAssessedHealthStatus, data = norm_patients)
summary(t2)
```

    ## 
    ## Call:
    ## lm(formula = Systolic ~ Age + Weight + Height + Gender + Location + 
    ##     Smoker + SelfAssessedHealthStatus, data = norm_patients)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -8.6277 -3.1293 -0.6898  3.1426 11.8390 
    ## 
    ## Coefficients:
    ##                                     Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)                         121.1615     1.8512  65.449  < 2e-16
    ## Age                                   0.5791     0.4834   1.198   0.2341
    ## Weight                               -0.3565     1.5510  -0.230   0.8187
    ## Height                                1.3321     0.7202   1.850   0.0677
    ## Gender'Male'                         -1.4794     3.2657  -0.453   0.6516
    ## Location'St. Mary's Medical Center'  -0.8565     1.2980  -0.660   0.5110
    ## Location'VA Hospital'                -1.7348     1.1332  -1.531   0.1293
    ## Smoker1                               9.6731     1.0459   9.249 1.15e-14
    ## SelfAssessedHealthStatus'Fair'       -2.7510     1.5106  -1.821   0.0720
    ## SelfAssessedHealthStatus'Good'        0.5864     1.1783   0.498   0.6200
    ## SelfAssessedHealthStatus'Poor'        0.4593     1.6762   0.274   0.7847
    ##                                        
    ## (Intercept)                         ***
    ## Age                                    
    ## Weight                                 
    ## Height                              .  
    ## Gender'Male'                           
    ## Location'St. Mary's Medical Center'    
    ## Location'VA Hospital'                  
    ## Smoker1                             ***
    ## SelfAssessedHealthStatus'Fair'      .  
    ## SelfAssessedHealthStatus'Good'         
    ## SelfAssessedHealthStatus'Poor'         
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 4.713 on 89 degrees of freedom
    ## Multiple R-squared:  0.5569, Adjusted R-squared:  0.5071 
    ## F-statistic: 11.19 on 10 and 89 DF,  p-value: 3.894e-12

### **Question 4**

The regression coefficients after Normalization are:
Age 0.5791068 vs 0.0802597
weight -0.3565445 vs -0.0134183
height 1.3320642 vs 0.4696206
gender Male -1.4793907 vs -1.4793907
Location 'St. Mary's Medical Center' -0.8565008 vs -0.8565008
Location 'VA Hospital' -1.7348405 vs -1.7348405
Smoker 9.6730871 vs 9.6730871
Self Assessed Health Status'Fair' -2.7509682 vs -2.7509682
Self Assessed Health Status'Good' 0.5863787 vs 0.5863787
Self Assessed Health Status'Poor' 0.4593428 vs 0.4593428

### **Question 5**

Not much changed but now as age goes up one blood pressure goes up 0.57, as weight goes up one blood pressure still goes down 0.35 that cant be right. A height goes up goes up one blood pressure goes up 1.33. Nothing changed from the fist model but the

### **Question 7**

#### I want to see if there are any outlies in this data.

``` r
par(mfrow=c(2,2))
plot(t2, which=1:4)
```

![](homework_2_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-10-1.png)

Looking at the Cooks Distance plot above it looks like row 93 could be an outliers.

### **Question 8**

Identify one or few useless features

``` r
avPlots(t2, id.n=2, id.cex=0.7)
```

![](homework_2_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-11-1.png)![](homework_2_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-11-2.png)

Looking at the summary from the model I am getting a odd result in height I don't think this would matter for blood pressure but it says that is should. If remove values based on p-value I would only keep Smoker. looking at the added valuable plot most a of the predictors are flat which also shows that they can be removed.
