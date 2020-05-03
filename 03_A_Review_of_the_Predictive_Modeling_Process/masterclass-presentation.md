R Notebook
================

## 3 A Review of the Predictive Modeling Process

<http://www.feat.engineering/review-predictive-modeling-process.html>

## Covers how to

  - measure model performance

  - use data well (e.g. splitting and resampling)

  - tune models

  - compare model performance

## Uses data

  - OkCupid Profile Data

  - Ames housing price

## OkCupid Profile Data

<https://github.com/rudeboybert/JSE_OkCupid>

Goal: Predict whether a person’s works in science, technology,
engineering, and math (STEM).

  - 50 000 profiles in San Francisco, USA

  - Most data is categorical.
    
      - They converted it to dummy variables.

  - STEM workers are infrequent (18.5%).
    
      - They “down-sampled” to ensure each class (e.g. STEM) has equal
        number of profiles.

## 3.2 Measuring Performance

<http://www.feat.engineering/measuring-performance.html>

## Metrics \> regression \> numeric

## Root Mean Squared Error (RMSE)

  - observations vs. pridictions: average distance

  - \[original\]

  - Good -\> RMSE \~ 0

## Coefficient of determination (R^2)

  - observations vs. pridictions: (standard correlation)^2

## Coefficient of determination (R^2): Pro

For linear models:

  - \~ How much variability can the model explain?

  - \[none\] (proportion):
    
      - Good -\> R^2 \~ 1
      - Bad -\> R^2 \~ 0

## Coefficient of determination (R^2): Con

  - can show very optimistic results when the outcome has large
    variance.

  - a handful of far predictions can artificially increase R^2.

  - Measures correlation not accuracy. (Main problem.)

## R^2 Measures correlation not accuracy

> a model could produce pridictions values that have a strong linear
> relationship with the observations values but the pridictions values
> do not conform to the 45-degree line of agreement.

## R^2 Measures correlation not accuracy

> E.g. when a model under-predicts at one extreme of the outcome and
> overpredicts at the other extreme of the outcome.

## R^2 problem: book example

<img src=http://i.imgur.com/lMFSHw2.png width=760>

## R^2 problem: my example

``` r
library(tidyverse)
#> ── Attaching packages ─────────── tidyverse 1.3.0 ──
#> ✓ ggplot2 3.3.0           ✓ purrr   0.3.4      
#> ✓ tibble  3.0.1           ✓ dplyr   0.8.99.9002
#> ✓ tidyr   1.0.2           ✓ stringr 1.4.0      
#> ✓ readr   1.3.1           ✓ forcats 0.5.0
#> ── Conflicts ────────────── tidyverse_conflicts() ──
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()

observations <- tibble::tibble(x = 1:10, outcome = 1:10)

p <- ggplot(observations, aes(x, outcome)) + 
  geom_point(data = observations) +
  geom_abline()
p
```

![](masterclass-presentation_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
pridictions <- tibble::tribble(
  ~x, ~outcome,
  # under-predict
  0,   2,  
  1,   3,
  2,   4,
  # over-predict
  8,   6,  
  9,   7,
  10,   8,
)

p + geom_point(data = pridictions, color = "red")
```

![](masterclass-presentation_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->
