
<!-- badges: start -->

[![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/aweek)](https://cran.r-project.org/package=aweek)
[![Codecov test
coverage](https://codecov.io/gh/zkamvar/aweek/branch/master/graph/badge.svg)](https://codecov.io/gh/zkamvar/aweek?branch=master)
[![Travis build
status](https://travis-ci.org/zkamvar/aweek.svg?branch=master)](https://travis-ci.org/zkamvar/aweek)

<!-- badges: end -->

# Welcome to the *aweek* package\!

This package will convert dates to
[epiweeks](http://www.cmmcp.org/epiweek.htm),
[isoweeks](http://en.wikipedia.org/wiki/ISO_week_date), and all others
in between with minimal overhead. Other packages such as
[lubridate](https://github.com/tidyverse/lubridate),
[ISOweek](https://cran.r-project.org/package=ISOweek), and
[surveillance](http://surveillance.r-forge.r-project.org/) have the
ability to define ISOweeks and/or epiweeks have the ability to define
ISOweeks and/or epiweeks, the ability to easily switch between day and
week intervals is only available for ISOweeks. However, the ability to
easily switch between day and week intervals is only available for
ISOweeks and all of the packages above have dependencies that require
compiled code.

``` r
library(aweek)
set.seed(2019-02-26)
onset <- Sys.Date() + sort(sample(-6:7, 20, replace = TRUE))
onset
```

    ##  [1] "2019-02-24" "2019-02-24" "2019-02-24" "2019-02-26" "2019-02-27"
    ##  [6] "2019-02-27" "2019-02-28" "2019-03-01" "2019-03-01" "2019-03-01"
    ## [11] "2019-03-03" "2019-03-04" "2019-03-05" "2019-03-05" "2019-03-06"
    ## [16] "2019-03-07" "2019-03-07" "2019-03-08" "2019-03-08" "2019-03-09"

``` r
date2week(onset, week_start = 1) # ISO weeks starting on Monday (default)
```

    ## <aweek start: Monday>
    ##  [1] "2019-W08-7" "2019-W08-7" "2019-W08-7" "2019-W09-2" "2019-W09-3"
    ##  [6] "2019-W09-3" "2019-W09-4" "2019-W09-5" "2019-W09-5" "2019-W09-5"
    ## [11] "2019-W09-7" "2019-W10-1" "2019-W10-2" "2019-W10-2" "2019-W10-3"
    ## [16] "2019-W10-4" "2019-W10-4" "2019-W10-5" "2019-W10-5" "2019-W10-6"

``` r
date2week(onset, week_start = 7) # EPI week starting on Sunday
```

    ## <aweek start: Sunday>
    ##  [1] "2019-W09-1" "2019-W09-1" "2019-W09-1" "2019-W09-3" "2019-W09-4"
    ##  [6] "2019-W09-4" "2019-W09-5" "2019-W09-6" "2019-W09-6" "2019-W09-6"
    ## [11] "2019-W10-1" "2019-W10-2" "2019-W10-3" "2019-W10-3" "2019-W10-4"
    ## [16] "2019-W10-5" "2019-W10-5" "2019-W10-6" "2019-W10-6" "2019-W10-7"

``` r
date2week(onset, week_start = 6) # EPI week starting on Saturday
```

    ## <aweek start: Saturday>
    ##  [1] "2019-W09-2" "2019-W09-2" "2019-W09-2" "2019-W09-4" "2019-W09-5"
    ##  [6] "2019-W09-5" "2019-W09-6" "2019-W09-7" "2019-W09-7" "2019-W09-7"
    ## [11] "2019-W10-2" "2019-W10-3" "2019-W10-4" "2019-W10-4" "2019-W10-5"
    ## [16] "2019-W10-6" "2019-W10-6" "2019-W10-7" "2019-W10-7" "2019-W11-1"

``` r
x <- date2week(onset, week_start = 6, floor_day = TRUE) # truncate to just the weeks
table(x)
```

    ## x
    ## 2019-W09 2019-W10 2019-W11 
    ##       10        9        1

``` r
table(week2date(x))
```

    ## 
    ## 2019-02-23 2019-03-02 2019-03-09 
    ##         10          9          1

You can also use it to automatically calculate factor levels, which is
useful in tabulating across weeks and including missing values.

``` r
set.seed(2019-02-26)
onset <- Sys.Date() + sort(sample(-48:49, 20, replace = TRUE))
x <- date2week(onset, week_start = 6, floor_day = TRUE, factor = TRUE)
x
```

    ## <aweek start: Saturday>
    ##  [1] 2019-W03 2019-W03 2019-W03 2019-W05 2019-W06 2019-W06 2019-W07
    ##  [8] 2019-W08 2019-W08 2019-W08 2019-W10 2019-W11 2019-W12 2019-W12
    ## [15] 2019-W14 2019-W14 2019-W14 2019-W15 2019-W15 2019-W16
    ## 14 Levels: 2019-W03 2019-W04 2019-W05 2019-W06 2019-W07 ... 2019-W16

``` r
table(x)
```

    ## x
    ## 2019-W03 2019-W04 2019-W05 2019-W06 2019-W07 2019-W08 2019-W09 2019-W10 
    ##        3        0        1        2        1        3        0        1 
    ## 2019-W11 2019-W12 2019-W13 2019-W14 2019-W15 2019-W16 
    ##        1        2        0        3        2        1

## Installing the package

To benefit from the latest features and bug fixes, install the
development, *github* version of the package using:

``` r
remotes::install_github("zkamvar/aweek")
```

Note that this requires the package *remotes* installed.

# Main Features

  - `date2week()` converts dates to a week format (YYYY-Www-d) that can
    start on any day.
  - `week2date()/as.Date()` does the backwards conversion from
    (YYYY-Www(-d)) to a numeric date.
  - Dependencies only on R itself.

## Getting help online

Bug reports and feature requests should be posted on *github* using the
[*issue*](http://github.com/reconhub/aweek/issues) system. All other
questions should be posted on the **RECON forum**: <br>
<http://www.repidemicsconsortium.org/forum/>

Contributions are welcome via **pull requests**.

Please note that this project is released with a [Contributor Code of
Conduct](CONDUCT.md). By participating in this project you agree to
abide by its terms.
