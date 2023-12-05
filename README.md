
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
easily manipulate it with {dplyr} or {purrr}. Later, you can rebuild a
Rmd or Qmd from the tibble.

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

``` r
file_out <- tempfile(fileext = ".Rmd")
out <- combine_tbl_to_file(tbl_rmd, file_out)
```

Read the file re-created with `combine_tbl_to_file()` to verify it is a
proper Rmd

```` r
cat(readLines(file_out), sep = "\n")
#> ---
#> title: dev_history.Rmd for working package
#> output: html_document
#> author: statnmap
#> date: '2023-10-12'
#> editor_options:
#>   chunk_output_type: console
#> ---
#> 
#> 
#> ```{r development}
#> #| include: no
#> 
#> library(testthat)
#> ```
#> 
#> 
#> <!--
#> # Description of your package
#> 
#> This will fill the description of your package.
#> -->
#> ```{r description}
#> # --> for parse tests
#> fusen::fill_description(
#>     pkg = here::here(),
#>     fields = list(
#>         Title = "Build A Package From Rmarkdown file",
#>         Description = "Use Rmarkdown First method to build your package. Start your package with documentation. Everything can be set from a Rmarkdown file in your project.",
#>         `Authors@R` = c(
#>             person("John", "Doe", email = "john@email.me", role = c("aut", "cre"), comment = c(ORCID = "0000-0000-0000-0000"))
#>         )
#>     )
#> )
#> # Define License with use_*_license()
#> usethis::use_mit_license("John Doe")
#> ```
#> 
#> 
#> # Read data
#> 
#> <!-- Store your dataset in a directory named "inst/" at the root of your project -->
#> <!-- Use it for your tests in this Rmd thanks to `load_all()` to make it available
#> and `system.file()` to read it in your examples 
#> -->
#> ```{r development-2}
#> # Set error=TRUE for checks() if needed
#> # Run all in the console directly
#> # Create "inst/" directory
#> dir.create(here::here("inst"))
#> # Example dataset
#> file.copy(system.file("nyc_squirrels_sample.csv", package = "fusen"), here::here("inst"))
#> ```
#> 
#> 
#> # Calculate the median of a vector
#> 
#> Here is some inline R code : `r 1+1`
#> ```{r function}
#> #' My median
#> #'
#> #' @param x Vector of Numeric values
#> #' @inheritParams stats::median
#> #'
#> #' @return
#> #' Median of vector x
#> #' @export
#> #'
#> #' @examples
#> #' my_median(2:20)
#> my_median <- function(x, na.rm = TRUE) {
#>     if (!is.numeric(x)) {
#>         stop("x should be numeric")
#>     }
#>     stats::median(x, na.rm = na.rm)
#> }
#> ```
#> 
#> 
#> ```{r examples}
#> my_median(1:12)
#> ```
#> 
#> 
#> ```{r tests}
#> test_that("my_median works properly and show error if needed", {
#>     expect_error(my_median("text"))
#> })
#> ```
#> 
#> 
#> # Calculate the mean of a vector
#> ## Use sub-functions in the same chunk
#> ```{r function-1}
#> #| filename: the_median_file
#> 
#> #' My Other median
#> #'
#> #' @param x Vector of Numeric values
#> #' @inheritParams stats::median
#> #'
#> #' @return
#> #' Median of vector x
#> #' @export
#> #'
#> #' @examples
#> my_other_median <- function(x, na.rm = TRUE) {
#>     if (!is.numeric(x)) {
#>         stop("x should be numeric")
#>     }
#>     sub_median(x, na.rm = na.rm)
#> }
#> 
#> #' Core of the median not exported
#> #' @param x Vector of Numeric values
#> #' @inheritParams stats::median
#> sub_median <- function(x, na.rm = TRUE) {
#>     stats::median(x, na.rm)
#> }
#> ```
#> 
#> 
#> ```{r examples-1}
#> my_other_median(1:12)
#> my_other_median(8:20)
#> my_other_median(20:50)
#> ```
#> 
#> 
#> ```{r tests-1}
#> test_that("my_median works properly and show error if needed", {
#>     expect_true(my_other_median(1:12) == 6.5)
#>     expect_error(my_other_median("text"))
#> })
#> ```
#> 
#> 
#> ```
#> not a R chunk that should be kept in vignette, but not in code
#> ```
#> 
#> ```{r development-1}
#> #| eval: no
#> 
#> # Run but keep eval=FALSE to avoid infinite loop
#> # Execute in the console directly
#> fusen::inflate(flat_file = "dev/dev_history.Rmd")
#> ```
#> 
#> 
#> 
#> # Inflate your package
#> ```{r}
#> # duplicate empty name
#> ```
#> 
#> 
#> ```{r}
#> # duplicate empty name
#> ```
#> 
#> 
#> 
#> You're one inflate from paper to box.
#> Build your package from this very Rmd using `fusen::inflate()`
#> 
#> - Verify your `"DESCRIPTION"` file has been updated
#> - Verify your function is in `"R/"` directory
#> - Verify your test is in `"tests/testthat/"` directory
#> - Verify this Rmd appears in `"vignettes/"` directory
````

## Similar work

{lightparser} is a light version of {parsermd} that has not been updated
for a long time, and which is not compatible with the latest versions of
C++ compilers. {lightparser} does not rely on C++ compilation.

## Code of Conduct

Please note that the lightparser project is released with a [Contributor
Code of
Conduct](https://contributor-covenant.org/version/2/1/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.
