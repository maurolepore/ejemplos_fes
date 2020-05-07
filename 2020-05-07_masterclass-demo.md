Chapter 3
================

# [A Review of the Predictive Modeling Process](http://www.feat.engineering/review-predictive-modeling-process.html)

Mauro, 2020-05-07

``` r
library(fs)
library(here)
#> here() starts at /home/mauro/git/ejemplos_fes
```

``` r
chapter_dir <- here::here("03_A_Review_of_the_Predictive_Modeling_Process")
r_files <- fs::dir_ls(chapter_dir, regexp = "[.]R$")
fs::path_file(r_files)
#> [1] "03_02_01_Regression_Metric.R"               
#> [2] "03_02_02_Classification_Metric.R"           
#> [3] "03_06_Model_Optimization_and_Tuning.R"      
#> [4] "03_06_Model_Optimization_and_Tuning_FIXME.R"
```

``` r
source(r_files[[1]])
#> Warning in readChar(con, 5L, useBytes = TRUE): cannot open compressed file '../
#> 01_Introduction/1_03_A_More_Complex_Example/lm_date_only.RData', probable reason
#> 'No such file or directory'
#> Error in readChar(con, 5L, useBytes = TRUE): cannot open the connection
warnings()
```

``` r
source(r_files[[2]])
#> Warning in readChar(con, 5L, useBytes = TRUE): cannot open compressed file '../
#> 05_Encoding_Categorical_Predictors/5_06_Creating_Features_from_Text_Data//
#> okc_glm_keyword.RData', probable reason 'No such file or directory'
#> Error in readChar(con, 5L, useBytes = TRUE): cannot open the connection
warnings()
```

``` r
source(r_files[[3]])
#> Warning in readChar(con, 5L, useBytes = TRUE): cannot open compressed file '../
#> 05_Encoding_Categorical_Predictors/5_06_Creating_Features_from_Text_Data//
#> okc_mlp_keyword.RData', probable reason 'No such file or directory'
#> Error in readChar(con, 5L, useBytes = TRUE): cannot open the connection
warnings()
```

## Search for missing data in Git’s log

No luck.

``` bash
LM_DATE_ONLY="01_Introduction/1_03_A_More_Complex_Example/lm_date_only.RData"
git log --name-only -- $LM_DATE_ONLY
```

Found this one\!

``` bash
OKC_KNN_KEYWORD="05_Encoding_Categorical_Predictors/5_06_Creating_Features_from_Text_Data//okc_knn_keyword.RData"
git log --name-only -- $OKC_KNN_KEYWORD
#> commit 3f522d743c01456efcff332b568269ce06432b1f
#> Author: Max Kuhn <mxkuhn@gmail.com>
#> Date:   Tue Jun 18 17:50:53 2019 -0400
#> 
#>     fixed naming inconsistency
#> 
#> 05_Encoding_Categorical_Predictors/5_06_Creating_Features_from_Text_Data/okc_knn_keyword.RData
#> 
#> commit 9d736d442e53778bafbbe0e324a4b3f930b37fe0
#> Author: Max Kuhn <mxkuhn@gmail.com>
#> Date:   Mon Jun 17 15:44:09 2019 -0400
#> 
#>     chapter 5 files
#> 
#> 05_Encoding_Categorical_Predictors/5_06_Creating_Features_from_Text_Data/okc_knn_keyword.RData
```

## Rerun with found data

``` r
patched_file <- fs::path(here::here(
  "03_A_Review_of_the_Predictive_Modeling_Process", 
  "03_06_Model_Optimization_and_Tuning_FIXME.R"
))
```

``` r
# Load pre-computed model results for OkC data
load(
  file.path(
    # FIXME: Restore missing data where the scripts expect it
    # "..",
    # "05_Encoding_Categorical_Predictors",
    # "5_06_Creating_Features_from_Text_Data/",
    "okc_knn_keyword.RData"
  )
)
```

``` r
source(patched_file)
#> Warning in readChar(con, 5L, useBytes = TRUE): cannot open compressed file '../
#> 05_Encoding_Categorical_Predictors/5_06_Creating_Features_from_Text_Data//
#> okc_mlp_keyword.RData', probable reason 'No such file or directory'
#> Error in readChar(con, 5L, useBytes = TRUE): cannot open the connection
warnings()
```

I give up.
