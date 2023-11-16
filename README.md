
<!-- README.md is generated from README.Rmd. Please edit that file -->

# lightparser

<!-- badges: start -->

[![R-CMD-check](https://github.com/ThinkR-open/lightparser/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/ThinkR-open/lightparser/actions/workflows/R-CMD-check.yaml)
[![Codecov test
coverage](https://codecov.io/gh/ThinkR-open/lightparser/branch/main/graph/badge.svg)](https://app.codecov.io/gh/ThinkR-open/lightparser?branch=main)
<!-- badges: end -->

You need to extract some specific information from your Rmd or Qmd file?
{lightparser} is designed to split your Rmd or Qmd file by sections into
a tibble: titles, text, chunks. It stores them as a tibble, so you can
easily manipulate it with dplyr or purrr. Later, you can rebuild a Rmd
or Qmd from the tibble.

This is a light version of {parsermd} that has not been updated for a
long time, and which is not compatible with the latest versions of C++
compilers. {lightparser} does not rely on C++ compilation.

## Installation

You can install the development version of lightparser from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("ThinkR-open/lightparser")
```

## Documentation

Full documentation website on:
<https://ThinkR-open.github.io/lightparser>

## Example

Split your Rmd or Qmd file into a tibble:

``` r
library(lightparser)

file <- system.file(
  "dev-template-parsing.Rmd",
  package = "lightparser"
)
tbl_rmd <- split_to_tbl(file)
tbl_rmd
#> # A tibble: 35 × 6
#>    type    label       params           text         code       heading         
#>    <chr>   <chr>       <list>           <named list> <list>     <chr>           
#>  1 yaml    <NA>        <named list [5]> <lgl [1]>    <lgl [1]>  <NA>            
#>  2 inline  <NA>        <lgl [1]>        <chr [0]>    <lgl [1]>  <NA>            
#>  3 block   development <named list [2]> <lgl [1]>    <chr [1]>  <NA>            
#>  4 inline  <NA>        <lgl [1]>        <chr [2]>    <lgl [1]>  <NA>            
#>  5 heading <NA>        <lgl [1]>        <chr [1]>    <lgl [1]>  Description of …
#>  6 inline  <NA>        <lgl [1]>        <chr [3]>    <lgl [1]>  <NA>            
#>  7 block   description <named list [1]> <lgl [1]>    <chr [13]> <NA>            
#>  8 inline  <NA>        <lgl [1]>        <chr [1]>    <lgl [1]>  <NA>            
#>  9 heading <NA>        <lgl [1]>        <chr [1]>    <lgl [1]>  Read data       
#> 10 inline  <NA>        <lgl [1]>        <chr [5]>    <lgl [1]>  <NA>            
#> # ℹ 25 more rows
```

Combine the tibble into a Rmd or Qmd file:

## Code of Conduct

Please note that the lightparser project is released with a [Contributor
Code of
Conduct](https://contributor-covenant.org/version/2/1/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.
