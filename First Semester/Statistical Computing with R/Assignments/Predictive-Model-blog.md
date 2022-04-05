# Validation & Cross-validation for Predictive Modelling including Linear Model as well as Multi Linear Model

Before starting topic let’s be familier on some term.

**Validation** : An act of confirming something as true or correct.
Also, Validation is the process of establishing documentary evidence
that a procedure, process, or activity was carried out in testing before
being put into production.

**Cross_Validation**: Cross-validation, also known as rotation
estimation or out-of-sample testing, is a set of model validation
procedures for determining how well the results of a statistical
investigation will generalize to new data.

**Linear Model**: The term “linear model” refers to a model that has a
linear relationship between the target variable and the independent
variable.

**Multi Linear Model**: A regression model that uses a straight line to
evaluate the connection between a quantitative dependent variable and
two or more independent variables is known as multiple linear
regression.

Here we will use R’s bulit in data `mtcars` for coding purpose. At first
let’s divided data into train set and test set in the ratio of 70% to
30%. While doing that task never forgot to use `seed()` function.

**seed()**: The random number generator is initialized using the seed()
method. To generate a random number, the random number generator
requires a starting value (seed value). The random number generator
defaults to using the current system time.

``` r
#Define the mtcars data as “data”:
data <- mtcars
#Use random seed to replicate the result
set.seed(123)
#Do random sampling to divide the cases into two independent samples
ind <- sample(2, nrow(mtcars), replace = T, prob = c(0.7, 0.3))
#Data partition
train.data <- data[ind==1,]
test.data <- data[ind==2,]
```

He divided our data into training and testing set in the ratio of 70 %
to 30%.

# Let’s fit Linear Model

Set mile per gallon(mpg) as dependent variable and weight(wt) as
independent variable.

``` r
lmodel <- lm(mpg~wt, data = train.data, method = "lm")
```

Let’s to model prediction.

``` r
pred <- predict(lmodel, data= test.data)
```

check value of R square and error value. To do at first we should load
`library(caret)` into our R studio.

``` r
library(caret)
```

    ## Loading required package: ggplot2

    ## Loading required package: lattice

``` r
pred <- predict(lmodel, data= test.data)
R2 <- R2(pred,  train.data$mpg)
R2
```

    ## [1] 0.7377021

Here we found value of R-square 73.77% that means 73.77% data fit the
linear model. Let’s check for error.

``` r
RMSE <- RMSE(pred, test.data$mpg)
```

    ## Warning in pred - obs: longer object length is not a multiple of shorter object
    ## length

``` r
RMSE
```

    ## [1] 8.786064

Hence error for the model is 12.6374.

# Leave-One-Out Cross-Validation approach

It’s usual practice when building a machine learning model to validate
your methods by setting aside a subset of your data as a test set.

LOOCV (leave-one-person-out cross validation) is a type of cross
validation that uses each individual as a “test” set. It’s a form of
k-fold cross validation in which the number of folds, k, equals the
number of participants in the dataset.

``` r
library(caret)
# Define training control
train.control <- trainControl(method = "LOOCV")
```

``` r
# Train the model
model1 <- train(mpg ~wt, data = mtcars, method = 
"lm",
trControl = train.control)
print(model1)
```

    ## Linear Regression 
    ## 
    ## 32 samples
    ##  1 predictor
    ## 
    ## No pre-processing
    ## Resampling: Leave-One-Out Cross-Validation 
    ## Summary of sample sizes: 31, 31, 31, 31, 31, 31, ... 
    ## Resampling results:
    ## 
    ##   RMSE      Rsquared   MAE     
    ##   3.201673  0.7104641  2.517436
    ## 
    ## Tuning parameter 'intercept' was held constant at a value of TRUE

``` r
pred1 <- predict(model1, test.data)
R2 <- R2(pred1, test.data$mpg)
R2
```

    ## [1] 0.7864736

We receive a value of R square of 78.46 percent when fitting the model
using the leave-one-out strategy, which is higher than the linear
regression model.

``` r
RMSE <- RMSE(pred1, test.data$mpg)
RMSE
```

    ## [1] 2.843768

Error is only 2.44 which is very lower than previous one.

# Let’s fit the model using K-folds Cross-Validation approach

A K-fold CV is one in which a given data set is divided into K
sections/folds, with each fold serving as a testing set at some point.
Let’s look at a 10-fold cross validation case (K=10). The data set is
divided into ten folds here. The first fold is used to test the model,
while the others are used to train it in the first iteration. The second
iteration uses the second fold as the testing set and the rest as the
training set. This procedure is repeated until each of the ten folds has
been utilized as a test set.

``` r
#k-fold cross validation
library(caret)
# Define training control
set.seed(123) 
train.control <- trainControl(method = "cv", number = 10)
# Train the model
model2 <- train(mpg ~ wt, data = train.data, method = 
"lm",
trControl = train.control)
```

Calculate value of R sqauere and error and observed is it will come
diffrerent from precious one.

``` r
library(caret)
pred2 <- predict(model2, train.data)
R2 <- R2(pred2, train.data$mpg)
R2
```

    ## [1] 0.7377021

This method gives the value of R square 73.77%. Which meand 73% data
fitted by the model.

# Fit the model using Repeated K-folds Cross-Validation approach

Repeated k-fold cross-validation is a technique for improving a machine
learning model’s predicted performance. Simply repeat the
cross-validation technique several times and return the mean result
across all folds from all runs.

``` r
#repeated k-fold cross validation
library(caret)
# Define training control
set.seed(123)
train.control <- trainControl(method = "repeatedcv", 
number = 10, repeats = 3)
# Train the model
model <- train(mpg ~wt, data = mtcars, method = 
"lm",
trControl = train.control)
# Summarize the results
print(model)
```

    ## Linear Regression 
    ## 
    ## 32 samples
    ##  1 predictor
    ## 
    ## No pre-processing
    ## Resampling: Cross-Validated (10 fold, repeated 3 times) 
    ## Summary of sample sizes: 28, 28, 29, 29, 29, 30, ... 
    ## Resampling results:
    ## 
    ##   RMSE      Rsquared   MAE     
    ##   2.975392  0.8351572  2.539797
    ## 
    ## Tuning parameter 'intercept' was held constant at a value of TRUE

Hence we get value of R- square 83.51% similarly value of RMSE 2.97.

# Summary: Which one should be used based on R-squared values of “lm” model?

-   R-square for training set: 0.7013

-   R-square for training with LOOCV: 0.7104641

-   R-square for training with k-folds CV: 0.7346939

-   R-square for training with repeated k-folds CV: 0.8351572

-   R-square for testing set: 0.9031085

-   R-square for testing with LOOCV: 0.9031085

-   R-square for testing with k-folds CV: 0.9031085

-   R-square for testing with repeated k-folds CV: 0.9031085

# Which one should be used based on RMSE value?

-   RMSE for training set: 3.08648

-   RMSE for training with LOOCV  
    3.201673

-   RMSE for training with k-folds CV: 2.85133

-   RMSE for training with repeated k- folds CV: 2.975392

-   RMSE for testing test: 2.279303

-   RMSE for testing with LOOCV: 2.244232

-   RMSE for testing with k-folds CV: 2.244232

-   RMSE for testing with repeated k- folds CV: 2.244232

# Let’s Repeate same Model for Multilinear Regression Model

It is an extension of the simple linear regression. Multiple linear
regression have more than one (two or more) independent variables.
Multiple linear regression has one (1) continuous dependent variable so
it is a supervised learning. All the assumptions of the simple linear
regression are also applicable here. There is one more condition.

Multicollinearity must not be present i.e. correlations between
independent variables must not be “high”

# Fitting Multi Linear Regression Model

``` r
mlr <- lm(mpg~., data = mtcars)
```

Let’s check variance inflection factor of `mlr`. The inflation factor is
the difference between the variance of estimating a parameter in a model
with many other factors and the variance of a model with only one term.
which is avilable in car packages.

``` r
library(car)
```

    ## Loading required package: carData

``` r
vif(mlr)
```

    ##       cyl      disp        hp      drat        wt      qsec        vs        am 
    ## 15.373833 21.620241  9.832037  3.374620 15.164887  7.527958  4.965873  4.648487 
    ##      gear      carb 
    ##  5.357452  7.908747

We need to drop the independent variable with highest VIF and run the
model again until all the VIF \<10!

``` r
#Removing “disp” variable:
mlr1 <- lm(mpg ~ cyl+hp+drat+wt+qsec+vs+am+gear+carb, data = mtcars)
vif(mlr)
```

    ##       cyl      disp        hp      drat        wt      qsec        vs        am 
    ## 15.373833 21.620241  9.832037  3.374620 15.164887  7.527958  4.965873  4.648487 
    ##      gear      carb 
    ##  5.357452  7.908747

``` r
#Removing “cyl” variable:
mlr2 <- lm(mpg ~ 
hp+drat+wt+qsec+vs+am+gear+carb, data = mtcars)
summary(mlr1)
```

    ## 
    ## Call:
    ## lm(formula = mpg ~ cyl + hp + drat + wt + qsec + vs + am + gear + 
    ##     carb, data = mtcars)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.7863 -1.4055 -0.2635  1.2029  4.4753 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept) 12.55052   18.52585   0.677   0.5052  
    ## cyl          0.09627    0.99715   0.097   0.9240  
    ## hp          -0.01295    0.01834  -0.706   0.4876  
    ## drat         0.92864    1.60794   0.578   0.5694  
    ## wt          -2.62694    1.19800  -2.193   0.0392 *
    ## qsec         0.66523    0.69335   0.959   0.3478  
    ## vs           0.16035    2.07277   0.077   0.9390  
    ## am           2.47882    2.03513   1.218   0.2361  
    ## gear         0.74300    1.47360   0.504   0.6191  
    ## carb        -0.61686    0.60566  -1.018   0.3195  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.623 on 22 degrees of freedom
    ## Multiple R-squared:  0.8655, Adjusted R-squared:  0.8105 
    ## F-statistic: 15.73 on 9 and 22 DF,  p-value: 1.183e-07

``` r
vif(mlr2)
```

    ##       hp     drat       wt     qsec       vs       am     gear     carb 
    ## 6.015788 3.111501 6.051127 5.918682 4.270956 4.285815 4.690187 4.290468

Now all Vif less than 10 so data is ready to fit different prediction
model.

# Leave-One-Out Cross-Validation approach on Multi Regression Model.

``` r
#Leave one out CV
library(caret)
# Define training control
train.control <- trainControl(method = "LOOCV")
# Train the model
mlr <- train(mpg ~ hp+drat+wt+qsec+vs+am+gear+carb, data = mtcars, method = "lm",
trControl = train.control)
# Summarize 
summary(mlr)
```

    ## 
    ## Call:
    ## lm(formula = .outcome ~ ., data = dat)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.8187 -1.3903 -0.3045  1.2269  4.5183 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept) 13.80810   12.88582   1.072   0.2950  
    ## hp          -0.01225    0.01649  -0.743   0.4650  
    ## drat         0.88894    1.52061   0.585   0.5645  
    ## wt          -2.60968    1.15878  -2.252   0.0342 *
    ## qsec         0.63983    0.62752   1.020   0.3185  
    ## vs           0.08786    1.88992   0.046   0.9633  
    ## am           2.42418    1.91227   1.268   0.2176  
    ## gear         0.69390    1.35294   0.513   0.6129  
    ## carb        -0.61286    0.59109  -1.037   0.3106  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.566 on 23 degrees of freedom
    ## Multiple R-squared:  0.8655, Adjusted R-squared:  0.8187 
    ## F-statistic:  18.5 on 8 and 23 DF,  p-value: 2.627e-08

We got value of R square is 86.55% value of error is 2.566 on 23 degree
of freedom.

# Let’s fit the model using K-folds Cross-Validation approach on Multi Linear Regression Model.

``` r
#K- folds Cross- Validation
library(caret)
# Define training control
train.control <- trainControl(method = "cv", number = 10)
# Train the model
mlr1<- train(mpg ~ hp+drat+wt+qsec+vs+am+gear+carb, data = mtcars, method = "lm",
trControl = train.control)
# Summarize 
summary(mlr1)
```

    ## 
    ## Call:
    ## lm(formula = .outcome ~ ., data = dat)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.8187 -1.3903 -0.3045  1.2269  4.5183 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept) 13.80810   12.88582   1.072   0.2950  
    ## hp          -0.01225    0.01649  -0.743   0.4650  
    ## drat         0.88894    1.52061   0.585   0.5645  
    ## wt          -2.60968    1.15878  -2.252   0.0342 *
    ## qsec         0.63983    0.62752   1.020   0.3185  
    ## vs           0.08786    1.88992   0.046   0.9633  
    ## am           2.42418    1.91227   1.268   0.2176  
    ## gear         0.69390    1.35294   0.513   0.6129  
    ## carb        -0.61286    0.59109  -1.037   0.3106  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.566 on 23 degrees of freedom
    ## Multiple R-squared:  0.8655, Adjusted R-squared:  0.8187 
    ## F-statistic:  18.5 on 8 and 23 DF,  p-value: 2.627e-08

Again, we got value of r square 86.55% similarly value for the error is
2.566.

# Fit the model using Repeated K-folds Cross-Validation approach

``` r
set.seed(224)
# Repeated K- folds Cross- Validation
library(caret)
# Define training control
train.control <- trainControl(method = "repeatedcv", 
number = 10, repeats = 3)
# Train the model
mlr2<- train(mpg ~ hp+drat+wt+qsec+vs+am+gear+carb, data = mtcars, method = "lm",
trControl = train.control)
# Summarize 
summary(mlr2)
```

    ## 
    ## Call:
    ## lm(formula = .outcome ~ ., data = dat)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.8187 -1.3903 -0.3045  1.2269  4.5183 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept) 13.80810   12.88582   1.072   0.2950  
    ## hp          -0.01225    0.01649  -0.743   0.4650  
    ## drat         0.88894    1.52061   0.585   0.5645  
    ## wt          -2.60968    1.15878  -2.252   0.0342 *
    ## qsec         0.63983    0.62752   1.020   0.3185  
    ## vs           0.08786    1.88992   0.046   0.9633  
    ## am           2.42418    1.91227   1.268   0.2176  
    ## gear         0.69390    1.35294   0.513   0.6129  
    ## carb        -0.61286    0.59109  -1.037   0.3106  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.566 on 23 degrees of freedom
    ## Multiple R-squared:  0.8655, Adjusted R-squared:  0.8187 
    ## F-statistic:  18.5 on 8 and 23 DF,  p-value: 2.627e-08

We got value for R square 86.55 % and value for error is 2.566.

                 Than you for Reading
