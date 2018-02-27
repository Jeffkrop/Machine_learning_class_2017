Jeff Kropelnicki
================
2018-02-27

``` r
patients <- read.csv("patients.csv")

norm  <- patients %>% select(Age, Height, Weight)
norm <- as.data.frame(scale(norm))
#patients$Smoker <- as.factor(patients$Smoker)

norm_patients <- patients[,-c(1,3,4,6,7,8,10)]

dummy <- patients %>% select(Gender, Location, SelfAssessedHealthStatus, Smoker)
dummy <- data.frame(model.matrix(~., data=dummy)[,-1])

norm_patients <- cbind(norm_patients, norm, dummy) %>%
                select(Systolic,
                       Age,
                       Height,
                       Weight,
                       Smoker,
                       male = Gender.Male., 
                       Marys = Location.St..Mary.s.Medical.Center., 
                       VA = Location.VA.Hospital., 
                       fair = SelfAssessedHealthStatus.Fair., 
                       good = SelfAssessedHealthStatus.Good., 
                       poor = SelfAssessedHealthStatus.Poor.)


t1 <- lm(Systolic ~ Age + Weight + Height + male + Marys + VA + fair + good + poor + Smoker, data = norm_patients)
#summary(t1)
```

``` r
library(glmnet)

# Data = considering that we have a data frame named dataF, with its first column being the class
x <- as.matrix(norm_patients[,-1]) # Removes Systolic from the dataframe and makes two matrix 
y <- as.double(as.matrix(norm_patients[, 1]))


set.seed(999)
cv.lasso <- cv.glmnet(x, y, alpha=1, nfolds = 10)

plot(cv.lasso)
```

![](homework_3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-1.png)

``` r
plot(cv.lasso$glmnet.fit, xvar="lambda", label = TRUE)
```

![](homework_3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-2.png)

Lets see what the lambdas are for the min and 1 standard deviation from the min are

``` r
#The min 
t1 <- cv.lasso$lambda.min

#1 SD from the min
t2 <- cv.lasso$lambda.1se
```

The minimum lambda is 0.3486696 and the lambda that is one standard deviation from the minimum is 1.8607449

Lets see what predictors are selected with the minimum lambda

``` r
#predictors at mim lambda
coef(cv.lasso, s=cv.lasso$lambda.min)
```

    ## 11 x 1 sparse Matrix of class "dgCMatrix"
    ##                       1
    ## (Intercept) 120.1790459
    ## Age           0.2870241
    ## Height        0.2224009
    ## Weight        .        
    ## Smoker        9.0972628
    ## male          .        
    ## Marys         .        
    ## VA           -0.6140228
    ## fair         -1.7661787
    ## good          .        
    ## poor          .

If I used the minimum lambda I would use 5 predictors, Age, Height, smoker, VA hospital and fair health.

Lets see what predictors are selected with lambda this is one standard deviation from the minimum.

``` r
#predictors at 1 sd from min lambda
coef(cv.lasso, s=cv.lasso$lambda.1se)
```

    ## 11 x 1 sparse Matrix of class "dgCMatrix"
    ##                      1
    ## (Intercept) 120.729471
    ## Age           .       
    ## Height        .       
    ## Weight        .       
    ## Smoker        6.030966
    ## male          .       
    ## Marys         .       
    ## VA            .       
    ## fair          .       
    ## good          .       
    ## poor          .

If I used the lambda that is one standard deviation from the minimum I would use one predictor, Smoker.

**Question 5**
Looking the the predictors from the lambda = minimum 0.3486 I see that the largest are smoker and Fair health. Meaning if I pick a lambda that is above the minimum I will get these two.

**Question 6**
Knowing the min lambda and the 1 SD lambda I know that I need to pick a lambda that is between those. I changed the number some and found that I can use lambda = 1 and I get the two predictors

``` r
my_coef <- coef(cv.lasso, s=1)
print(my_coef)
```

    ## 11 x 1 sparse Matrix of class "dgCMatrix"
    ##                      1
    ## (Intercept) 120.121985
    ## Age           .       
    ## Height        .       
    ## Weight        .       
    ## Smoker        7.847697
    ## male          .       
    ## Marys         .       
    ## VA            .       
    ## fair         -0.068012
    ## good          .       
    ## poor          .

With lambda equal to one I get two predictors smoker and fair health status.

**Question 7**

``` r
t3 <- data.frame(name = my_coef@Dimnames[[1]][my_coef@i + 1], coefficient = my_coef@x)
print(t3)
```

    ##          name coefficient
    ## 1 (Intercept)  120.121985
    ## 2      Smoker    7.847697
    ## 3        fair   -0.068012

The coefficient for smoker is 7.847697 and the coefficient for fair health status is -0.068012.
