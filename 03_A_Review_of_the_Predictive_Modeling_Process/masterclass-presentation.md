Chapter 3
================
Mauro

# A Review of the Predictive Modeling Process

<http://www.feat.engineering/review-predictive-modeling-process.html>

## Overview

### 1\. Measure model performance

#### 1.1. Numeric variables

Measure regression between observed and predicted outcome

  - RMSE: Root Mean Squared Error

  - R^2: Coefficient of determination

#### 1.2. Discrete variables: Classification

  - Black or white

  - Probability

### 2\. Use data well (e.g. splitting and resampling)

### 3\. Tune models

### 4\. Compare model performance

# Measuring performance

# Measuring performance of Numeric variables

## Measuring performance: Numeric variables

  - Root Mean Squared Error (RMSE)

  - Coefficient of determination (R^2)

**Spoiler: Both very sensitive to extreeme values**

  - Good to predict the rank of the response

  - Not so good to predict the response itself

Robust approaches:

  - transform the data to reduce the impact of extreeme values
    (e.g. rank order)

Not covered, just mentioned:

  - median absolute deviation (MAD)

  - absolute error.

## RMSE | Root Mean Squared Error

**Spoiler: Use it\!**

  - actual vs. predicted: average distance

  - \[original\]

  - Good -\> RMSE \~ 0

## R^2 | Coefficient of determination

**Spoiler: Don’t use it; prefer RMSE\!**

  - actual vs. predicted: (standard correlation)^2

## R^2 | Coefficient of determination: Pro

For linear models:

  - \~ How much variability can the model explain?

  - \[none\] (proportion):
    
      - Good -\> R^2 \~ 1
      - Bad -\> R^2 \~ 0

## R^2 | Coefficient of determination: Con

  - can show very optimistic results when the y has large variance.

  - a handful of far predicted can artificially increase R^2.

  - Measures correlation not accuracy. (Main problem.)

## R^2 Measures correlation not accuracy

> a model could produce predicted values that have a strong linear
> relationship with the actual values but the predicted values do not
> conform to the 45-degree line of agreement.

## R^2 Measures correlation not accuracy

> E.g. when a model under-predicts at one extreme of the y and
> overpredicts at the other extreme of the y.

## R^2 problem: book example

(I don’t get it)

<img src=http://i.imgur.com/lMFSHw2.png width=760>

## Concordance Correlation Coefficient (CCC)

  - CCC penalizes R^2 for its bias (R^2 \* bias)

## 

# Measuring performance of discrete variables

## Measuring performance of discrete variables

Confusion matrix

<img src=http://i.imgur.com/BzD94hW.png width=400>

### Balanced: Accuracy

Proportion of correctly predicted = correct / total

### Imbalanced: Kappa

Normalizes the error rate to what would be expected by chance.

### Mozaic plot

<img src=http://i.imgur.com/sWYDjOC.png width=760>

### 2 classes

#### Hard classes (1 or 0)

  - Sensitivity (\#1\~1 / \#1) & Specificity (\#0\~0 / \#0)

  - Precision (\#1\~1 / \#\~1) & recall = Sensitivity

ASK: Mentions Bayesian statistics in “hard classes”, why?

#### Soft classes (probability or 1)

<img src=http://i.imgur.com/KTtXeEH.png width=760>

# Appendix

## R^2 problem: my example

``` r
library(tidyverse)
#> ── Attaching packages ────────── tidyverse 1.3.0 ──
#> ✓ ggplot2 3.3.0           ✓ purrr   0.3.4      
#> ✓ tibble  3.0.1           ✓ dplyr   0.8.99.9002
#> ✓ tidyr   1.0.2           ✓ stringr 1.4.0      
#> ✓ readr   1.3.1           ✓ forcats 0.5.0
#> ── Conflicts ───────────── tidyverse_conflicts() ──
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()

actual <- tibble::tibble(x = 1:10, y = 1:10)

p <- ggplot(actual, aes(x, y)) + 
  geom_point(data = actual) +
  geom_abline()
p
```

![](masterclass-presentation_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
predicted <- tibble::tribble(
  ~x, ~y,
  # under-predict
  0,   2,  
  1,   3,
  2,   4,
  # over-predict
  8,   6,  
  9,   7,
  10,   8,
)

p + geom_point(data = predicted, color = "red")
```

![](masterclass-presentation_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
compare <- inner_join(
  actual, predicted, 
  by = "x", 
  suffix = c("_actual", "_predicted")
)

compare
#> # A tibble: 5 x 3
#>       x y_actual y_predicted
#>   <dbl>    <int>       <dbl>
#> 1     1        1           3
#> 2     2        2           4
#> 3     8        8           6
#> 4     9        9           7
#> 5    10       10           8
```

``` r
compare %>% 
  ggplot(aes(y_actual, y_predicted)) +
  xlim(0, 10) +
  ylim(0, 10) +
  geom_abline(slope = 1) +
  geom_point() +
  geom_smooth(method = "lm")
#> `geom_smooth()` using formula 'y ~ x'
```

![](masterclass-presentation_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->
