
<!-- README.md is generated from README.Rmd. Please edit that file -->
profile
=======

[![Travis build status](https://travis-ci.org/creggian/profile.svg?branch=buildfix)](https://travis-ci.org/creggian/profile) [![Coverage status](https://codecov.io/gh/r-prof/profile/branch/master/graph/badge.svg)](https://codecov.io/github/r-prof/profile?branch=master) [![CRAN status](http://www.r-pkg.org/badges/version/profile)](https://cran.r-project.org/package=profile)

The goal of profile is to read and write files that contain run time profiling data. Currently, *profile* supports:

-   Files created by [`Rprof()`](https://www.rdocumentation.org/packages/utils/versions/3.4.3/topics/Rprof)
-   `.proto` files written by [`pprof -proto`](https://github.com/google/pprof), these can also be read by `pprof`

The data is available to the user for inspection and manipulation in a [documented stable data format](https://r-prof.github.io/profile/reference/validate_profile.html).

Installation
------------

You can install profile from GitHub with:

``` r
# install.packages("remotes")
remotes::install_github("r-prof/profile")
```

Example
-------

This simple example converts an `.out` file generated by `Rprof()` to the `.proto` format understood by `pprof`.

``` r
rprof_path <- tempfile("profile", fileext = ".out")
Rprof(rprof_path, line.profiling = TRUE)
x <- runif(1e6)
res <- vapply(x, function(x) if (x < 0.5) sqrt(x) else x * x, numeric(1))
Rprof(NULL)

library(profile)
ds <- read_rprof(rprof_path)
ds
#> Profile data: 33 samples
names(ds)
#> [1] "meta"         "sample_types" "samples"      "locations"   
#> [5] "functions"    ".rprof"
write_pprof(ds, file.path(tempdir(), "1.pb.gz"))
```
