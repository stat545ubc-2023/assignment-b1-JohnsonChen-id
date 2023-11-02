Assignment B1
================
2023-10-31

Here lies the main body of our new function:

``` r
#' Mean of variables in dataframe
#' 
#' Take the mean of the numerical variables from the original dataframe and return a named list
#'
#' @param dataf a dataframe with some numerical variables. The name dataf is used here to short for dataframe
#' @return a named list with only the mean of numerical variables
#'
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
mean_dataset <- function(dataf) {
  stopifnot(is.data.frame(dataf))
  return(dataf %>% select(where(is.numeric)) %>% sapply(mean, na.rm = TRUE))
}
```

Here are some examples using this function: Example 1:

``` r
mean_dataset(mtcars)
```

    ##        mpg        cyl       disp         hp       drat         wt       qsec 
    ##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250  17.848750 
    ##         vs         am       gear       carb 
    ##   0.437500   0.406250   3.687500   2.812500

Here we shall see that the function produced a named list for all
variables in the `mtcars` dataset. Since the dataset `mtcars` has all
its variables as numeric variables, the function outputs mean of all
variables.

Example 2:

``` r
head(sleep)
```

    ##   extra group ID
    ## 1   0.7     1  1
    ## 2  -1.6     1  2
    ## 3  -0.2     1  3
    ## 4  -1.2     1  4
    ## 5  -0.1     1  5
    ## 6   3.4     1  6

``` r
mean_dataset(sleep)
```

    ## extra 
    ##  1.54

As shown in the `sleep` dataset, the factor type variables are not shown
in the function output. Only the numeric variables are calculated.

Example 3:

``` r
factor_dataf <- data.frame(levels = c("High", "Low"), response = c("Well", "Poor"))
mean_dataset(factor_dataf)
```

    ## named list()

As shown in the simple`factor_dataf` dataset without any numerical
variable, an empty named list will be returned.

Example 4:

``` r
mean_dataset(3)
```

    ## Error in mean_dataset(3): is.data.frame(dataf) is not TRUE

As shown in this example, any input not the type of dataframe will cause
an error.

Testing:

We would use some tests to varify the function’s functionality:

``` r
library(testthat)
```

    ## 
    ## Attaching package: 'testthat'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     matches

    ## The following object is masked from 'package:purrr':
    ## 
    ##     is_null

    ## The following objects are masked from 'package:readr':
    ## 
    ##     edition_get, local_edition

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     matches

``` r
expect_equal(c(passing = 2), mean_dataset(data.frame(passing = c(1,1,3,3))))
expect_equal(c(passing = 2), mean_dataset(data.frame(passing = c(1,2, NA, 3))))
expect_equal(c(passing = 2), mean_dataset(data.frame(passing = c(1,1,3,3), frac = c("First", "Second", "Third", "Fourth"))))
expect_error(mean_dataset("545B"))
expect_length(mean_dataset(data.frame(passing = c())),0)
```
