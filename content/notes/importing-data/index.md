---
title: "Importing data into R"
date: 2019-03-01

type: docs
toc: true
draft: false
aliases: ["/datawrangle_import_functions.html"]
categories: ["datawrangle"]

menu:
  notes:
    parent: Data wrangling
    weight: 6
---




```r
library(tidyverse)
library(here)
theme_set(theme_minimal())

# set seed for reproducibility
set.seed(1234)
```

## `readr` vs. base R



One of the main advantages of `readr` functions over base R functions is that [they are typically much faster](http://r4ds.had.co.nz/data-import.html#compared-to-base-r). For example, let's import a randomly generated CSV file with 5,000 rows and 4 columns. How long does it take `read.csv()` to import this file vs. `readr::read_csv()`? To assess the differences, we use the [`microbenchmark`](https://cran.r-project.org/web/packages/microbenchmark/) to run each function 100 times and compare the distributions of the time it takes to complete the data import:


```r
library(microbenchmark)

results_small <- microbenchmark(
  read.csv = read.csv(file = here("static", "data", "sim-data-small.csv")),
  read_csv = read_csv(file = here("static", "data", "sim-data-small.csv"))
)
```


```r
autoplot(object = results_small) +
  scale_y_log10() +
  labs(y = "Time [milliseconds], logged")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/compare-speed-small-plot-1.png" width="672" />

`read_csv()` is over 5 times faster than `read.csv()`. Of course with relatively small data files, this isn't a large difference in absolute terms (a difference of 100 milliseconds is only .1 second). However, as the data file increases in size the performance savings will be much larger. Consider the same test with a CSV file with 500,000 rows:


```r
library(microbenchmark)

results_large <- microbenchmark(
  read.csv = read.csv(file = here("static", "data", "sim-data-large.csv")),
  read_csv = read_csv(file = here("static", "data", "sim-data-large.csv"))
)
```


```r
autoplot(object = results_large) +
  scale_y_log10() +
  labs(y = "Time [milliseconds], logged")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/compare-speed-large-plot-1.png" width="672" />

Here `read_csv()` is far superior to `read.csv()`.

## `vroom`

[`vroom`](https://vroom.r-lib.org/) is a recently developed package designed specifically for **speed**. It contains one of the fastest functions to import plain-text data files. Its syntax is similar to `readr::read_*()` functions, but works much more quickly.


```r
results_vroom <- microbenchmark(
  read.csv = read.csv(file = here("static", "data", "sim-data-large.csv")),
  read_csv = read_csv(file = here("static", "data", "sim-data-large.csv")),
  vroom = vroom::vroom(file = here("static", "data", "sim-data-large.csv"))
)
```


```r
autoplot(object = results_vroom) +
  scale_y_log10() +
  labs(y = "Time [milliseconds], logged")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/vroom-compare-speed-large-plot-1.png" width="672" />

## Alternative file formats

CSV files, while common, are not the only type of data storage format you will encounter in the wild. Here is a quick primer on other file formats you may encounter and how to import/export them using R. We'll use the `challenge` dataset in `readr` to demonstrate some of these formats.


```r
challenge <- read_csv(
  readr_example(file = "challenge.csv"), 
  col_types = cols(
    x = col_double(),
    y = col_date()
  )
)

challenge
```

```
## # A tibble: 2,000 x 2
##        x y         
##    <dbl> <date>    
##  1   404 NA        
##  2  4172 NA        
##  3  3004 NA        
##  4   787 NA        
##  5    37 NA        
##  6  2332 NA        
##  7  2489 NA        
##  8  1449 NA        
##  9  3665 NA        
## 10  3863 NA        
## # … with 1,990 more rows
```

## RDS

**RDS** is a custom binary format used exclusively by R to store data objects.


```r
# write to csv
write_csv(x = challenge, file = here("static", "data", "challenge.csv"))

# write to/read from rds
write_rds(x = challenge, file = here("static", "data", "challenge.csv"))
read_rds(file = here("static", "data", "challenge.csv"))
```

```
## # A tibble: 2,000 x 2
##        x y         
##    <dbl> <date>    
##  1   404 NA        
##  2  4172 NA        
##  3  3004 NA        
##  4   787 NA        
##  5    37 NA        
##  6  2332 NA        
##  7  2489 NA        
##  8  1449 NA        
##  9  3665 NA        
## 10  3863 NA        
## # … with 1,990 more rows
```

```r
# compare file size
file.info(here("static", "data", "challenge.rds"))$size %>%
  utils:::format.object_size("auto")
```

```
## [1] "11.5 Kb"
```

```r
file.info(here("static", "data", "challenge.csv"))$size %>%
  utils:::format.object_size("auto")
```

```
## [1] "31.9 Kb"
```

```r
# compare read speeds
microbenchmark(
  read_csv = read_csv(
    file = readr_example("challenge.csv"), 
    col_types = cols(
      x = col_double(),
      y = col_date()
    )
  ),
  read_rds = read_rds(file = here("static", "data", "challenge.rds"))
) %>%
  autoplot +
  labs(y = "Time [microseconds], logged")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/rds-1.png" width="672" />

By default, `write_rds()` does not compress the `.rds` file; use the `compress` argument to implement one of several different compression algorithms. `read_rds()` is noticably faster than `read_csv()`, and also has the benefit of [preserving column types](http://r4ds.had.co.nz/data-import.html#writing-to-a-file). The downside is that RDS is only implemented by R; it is not used by any other program so if you need to import/export data files into other languages like Python (or open in Excel), RDS is not a good storage format.

## `arrow`

The `arrow` package implements a binary file format called `feather` that is cross-compatible with many different programming languages:


```r
library(arrow)

write_feather(x = challenge, sink = here("static", "data", "challenge.feather"))
read_feather(file = here("static", "data", "challenge.feather"))
```

```
## # A tibble: 2,000 x 2
##        x y         
##    <dbl> <date>    
##  1   404 NA        
##  2  4172 NA        
##  3  3004 NA        
##  4   787 NA        
##  5    37 NA        
##  6  2332 NA        
##  7  2489 NA        
##  8  1449 NA        
##  9  3665 NA        
## 10  3863 NA        
## # … with 1,990 more rows
```

```r
# compare read speeds
microbenchmark(
  read_csv = read_csv(
    file = readr_example("challenge.csv"), 
    col_types = cols(
      x = col_double(),
      y = col_date()
    )
  ),
  read_rds = read_rds(file = here("static", "data", "challenge.rds")),
  read_feather = read_feather(file = here("static", "data", "challenge.feather"))
) %>%
  autoplot +
  scale_y_continuous(labels = scales::comma) +
  labs(y = "Time [microseconds], logged")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/feather-1.png" width="672" />

`read_feather()` is generally faster than RDS and `read_csv()`.^[Notice that the x-axis is logarithmically scaled.] Furthermore, [it has native support for Python, R, and Julia.](http://arrow.apache.org/blog/2019/08/08/r-package-on-cran/), so if you develop an analytics pipeline that switches between R and Python, you can import/export data files in `.feather` without any loss of information.

## `readxl`

[`readxl`](http://readxl.tidyverse.org/) enables you to read (but not write) Excel files (`.xls` and `xlsx`).^[If you need to export data into Excel, use `readr::write_csv_excel()`.]


```r
library(readxl)

xlsx_example <- readxl_example(path = "datasets.xlsx")
read_excel(xlsx_example)
```

```
## # A tibble: 150 x 5
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>  
##  1          5.1         3.5          1.4         0.2 setosa 
##  2          4.9         3            1.4         0.2 setosa 
##  3          4.7         3.2          1.3         0.2 setosa 
##  4          4.6         3.1          1.5         0.2 setosa 
##  5          5           3.6          1.4         0.2 setosa 
##  6          5.4         3.9          1.7         0.4 setosa 
##  7          4.6         3.4          1.4         0.3 setosa 
##  8          5           3.4          1.5         0.2 setosa 
##  9          4.4         2.9          1.4         0.2 setosa 
## 10          4.9         3.1          1.5         0.1 setosa 
## # … with 140 more rows
```

The nice thing about `readxl` is that you can access multiple sheets from the workbook. List the sheet names with `excel_sheets()`:


```r
excel_sheets(path = xlsx_example)
```

```
## [1] "iris"     "mtcars"   "chickwts" "quakes"
```

Then specify which worksheet you want by name or number:


```r
read_excel(path = xlsx_example, sheet = "chickwts")
```

```
## # A tibble: 71 x 2
##    weight feed     
##     <dbl> <chr>    
##  1    179 horsebean
##  2    160 horsebean
##  3    136 horsebean
##  4    227 horsebean
##  5    217 horsebean
##  6    168 horsebean
##  7    108 horsebean
##  8    124 horsebean
##  9    143 horsebean
## 10    140 horsebean
## # … with 61 more rows
```

```r
read_excel(path = xlsx_example, sheet = 2)
```

```
## # A tibble: 32 x 11
##      mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
##  1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
##  2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
##  3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
##  4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
##  5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
##  6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
##  7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
##  8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
##  9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
## 10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
## # … with 22 more rows
```

## `haven`

[`haven`](http://haven.tidyverse.org/) allows you to read and write data from other statistical packages such as SAS (`.sas7bdat` + `.sas7bcat`), SPSS (`.sav` + `.por`), and Stata (`.dta`).


```r
library(haven)

# SAS
read_sas(data_file = system.file("examples", "iris.sas7bdat", package = "haven"))
```

```
## # A tibble: 150 x 5
##    Sepal_Length Sepal_Width Petal_Length Petal_Width Species
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>  
##  1          5.1         3.5          1.4         0.2 setosa 
##  2          4.9         3            1.4         0.2 setosa 
##  3          4.7         3.2          1.3         0.2 setosa 
##  4          4.6         3.1          1.5         0.2 setosa 
##  5          5           3.6          1.4         0.2 setosa 
##  6          5.4         3.9          1.7         0.4 setosa 
##  7          4.6         3.4          1.4         0.3 setosa 
##  8          5           3.4          1.5         0.2 setosa 
##  9          4.4         2.9          1.4         0.2 setosa 
## 10          4.9         3.1          1.5         0.1 setosa 
## # … with 140 more rows
```

```r
write_sas(data = mtcars, path = here("static", "data", "mtcars.sas7bdat"))

# SPSS
read_sav(file = system.file("examples", "iris.sav", package = "haven"))
```

```
## # A tibble: 150 x 5
##    Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
##           <dbl>       <dbl>        <dbl>       <dbl>  <dbl+lbl>
##  1          5.1         3.5          1.4         0.2 1 [setosa]
##  2          4.9         3            1.4         0.2 1 [setosa]
##  3          4.7         3.2          1.3         0.2 1 [setosa]
##  4          4.6         3.1          1.5         0.2 1 [setosa]
##  5          5           3.6          1.4         0.2 1 [setosa]
##  6          5.4         3.9          1.7         0.4 1 [setosa]
##  7          4.6         3.4          1.4         0.3 1 [setosa]
##  8          5           3.4          1.5         0.2 1 [setosa]
##  9          4.4         2.9          1.4         0.2 1 [setosa]
## 10          4.9         3.1          1.5         0.1 1 [setosa]
## # … with 140 more rows
```

```r
write_sav(data = mtcars, path = here("static", "data", "mtcars.sav"))

# Stata
read_dta(file = system.file("examples", "iris.dta", package = "haven"))
```

```
## # A tibble: 150 x 5
##    sepallength sepalwidth petallength petalwidth species
##          <dbl>      <dbl>       <dbl>      <dbl> <chr>  
##  1        5.10       3.5         1.40      0.200 setosa 
##  2        4.90       3           1.40      0.200 setosa 
##  3        4.70       3.20        1.30      0.200 setosa 
##  4        4.60       3.10        1.5       0.200 setosa 
##  5        5          3.60        1.40      0.200 setosa 
##  6        5.40       3.90        1.70      0.400 setosa 
##  7        4.60       3.40        1.40      0.300 setosa 
##  8        5          3.40        1.5       0.200 setosa 
##  9        4.40       2.90        1.40      0.200 setosa 
## 10        4.90       3.10        1.5       0.100 setosa 
## # … with 140 more rows
```

```r
write_dta(data = mtcars, path = here("static", "data", "mtcars.dta"))
```

That said, if you can obtain your data file in a plain `.csv` or `.tsv` file format, **I strongly recommend it**. SAS, SPSS, and Stata files represent labeled data and missing values differently from R. `haven` attempts to bridge the gap and preserve as much information as possible, but I frequently find myself stripping out all the label information and rebuilding it using `dplyr` functions and the codebook for the data file.

{{% callout note %}}

Need to import a SAS, SPSS, or Stata data file? Read [the documentation](http://haven.tidyverse.org/articles/semantics.html) to learn how to best handle value labels and missing values.

{{% /callout %}}

## Session Info



```r
devtools::session_info()
```

```
## ─ Session info ───────────────────────────────────────────────────────────────
##  setting  value                       
##  version  R version 4.0.3 (2020-10-10)
##  os       macOS Catalina 10.15.7      
##  system   x86_64, darwin17.0          
##  ui       X11                         
##  language (EN)                        
##  collate  en_US.UTF-8                 
##  ctype    en_US.UTF-8                 
##  tz       America/Chicago             
##  date     2021-01-28                  
## 
## ─ Packages ───────────────────────────────────────────────────────────────────
##  package     * version date       lib source                              
##  assertthat    0.2.1   2019-03-21 [1] CRAN (R 4.0.0)                      
##  backports     1.2.1   2020-12-09 [1] CRAN (R 4.0.2)                      
##  blogdown      1.1     2021-01-19 [1] CRAN (R 4.0.3)                      
##  bookdown      0.21    2020-10-13 [1] CRAN (R 4.0.2)                      
##  broom         0.7.3   2020-12-16 [1] CRAN (R 4.0.2)                      
##  callr         3.5.1   2020-10-13 [1] CRAN (R 4.0.2)                      
##  cellranger    1.1.0   2016-07-27 [1] CRAN (R 4.0.0)                      
##  cli           2.2.0   2020-11-20 [1] CRAN (R 4.0.2)                      
##  colorspace    2.0-0   2020-11-11 [1] CRAN (R 4.0.2)                      
##  crayon        1.3.4   2017-09-16 [1] CRAN (R 4.0.0)                      
##  DBI           1.1.0   2019-12-15 [1] CRAN (R 4.0.0)                      
##  dbplyr        2.0.0   2020-11-03 [1] CRAN (R 4.0.2)                      
##  desc          1.2.0   2018-05-01 [1] CRAN (R 4.0.0)                      
##  devtools      2.3.2   2020-09-18 [1] CRAN (R 4.0.2)                      
##  digest        0.6.27  2020-10-24 [1] CRAN (R 4.0.2)                      
##  dplyr       * 1.0.2   2020-08-18 [1] CRAN (R 4.0.2)                      
##  ellipsis      0.3.1   2020-05-15 [1] CRAN (R 4.0.0)                      
##  evaluate      0.14    2019-05-28 [1] CRAN (R 4.0.0)                      
##  fansi         0.4.1   2020-01-08 [1] CRAN (R 4.0.0)                      
##  forcats     * 0.5.0   2020-03-01 [1] CRAN (R 4.0.0)                      
##  fs            1.5.0   2020-07-31 [1] CRAN (R 4.0.2)                      
##  generics      0.1.0   2020-10-31 [1] CRAN (R 4.0.2)                      
##  ggplot2     * 3.3.3   2020-12-30 [1] CRAN (R 4.0.2)                      
##  glue          1.4.2   2020-08-27 [1] CRAN (R 4.0.2)                      
##  gtable        0.3.0   2019-03-25 [1] CRAN (R 4.0.0)                      
##  haven         2.3.1   2020-06-01 [1] CRAN (R 4.0.0)                      
##  here        * 1.0.1   2020-12-13 [1] CRAN (R 4.0.2)                      
##  hms           0.5.3   2020-01-08 [1] CRAN (R 4.0.0)                      
##  htmltools     0.5.1   2021-01-12 [1] CRAN (R 4.0.2)                      
##  httr          1.4.2   2020-07-20 [1] CRAN (R 4.0.2)                      
##  jsonlite      1.7.2   2020-12-09 [1] CRAN (R 4.0.2)                      
##  knitr         1.30    2020-09-22 [1] CRAN (R 4.0.2)                      
##  lifecycle     0.2.0   2020-03-06 [1] CRAN (R 4.0.0)                      
##  lubridate     1.7.9.2 2021-01-18 [1] Github (tidyverse/lubridate@aab2e30)
##  magrittr      2.0.1   2020-11-17 [1] CRAN (R 4.0.2)                      
##  memoise       1.1.0   2017-04-21 [1] CRAN (R 4.0.0)                      
##  modelr        0.1.8   2020-05-19 [1] CRAN (R 4.0.0)                      
##  munsell       0.5.0   2018-06-12 [1] CRAN (R 4.0.0)                      
##  pillar        1.4.7   2020-11-20 [1] CRAN (R 4.0.2)                      
##  pkgbuild      1.2.0   2020-12-15 [1] CRAN (R 4.0.2)                      
##  pkgconfig     2.0.3   2019-09-22 [1] CRAN (R 4.0.0)                      
##  pkgload       1.1.0   2020-05-29 [1] CRAN (R 4.0.0)                      
##  prettyunits   1.1.1   2020-01-24 [1] CRAN (R 4.0.0)                      
##  processx      3.4.5   2020-11-30 [1] CRAN (R 4.0.2)                      
##  ps            1.5.0   2020-12-05 [1] CRAN (R 4.0.2)                      
##  purrr       * 0.3.4   2020-04-17 [1] CRAN (R 4.0.0)                      
##  R6            2.5.0   2020-10-28 [1] CRAN (R 4.0.2)                      
##  Rcpp          1.0.6   2021-01-15 [1] CRAN (R 4.0.2)                      
##  readr       * 1.4.0   2020-10-05 [1] CRAN (R 4.0.2)                      
##  readxl        1.3.1   2019-03-13 [1] CRAN (R 4.0.0)                      
##  remotes       2.2.0   2020-07-21 [1] CRAN (R 4.0.2)                      
##  reprex        0.3.0   2019-05-16 [1] CRAN (R 4.0.0)                      
##  rlang         0.4.10  2020-12-30 [1] CRAN (R 4.0.2)                      
##  rmarkdown     2.6     2020-12-14 [1] CRAN (R 4.0.2)                      
##  rprojroot     2.0.2   2020-11-15 [1] CRAN (R 4.0.2)                      
##  rstudioapi    0.13    2020-11-12 [1] CRAN (R 4.0.2)                      
##  rvest         0.3.6   2020-07-25 [1] CRAN (R 4.0.2)                      
##  scales        1.1.1   2020-05-11 [1] CRAN (R 4.0.0)                      
##  sessioninfo   1.1.1   2018-11-05 [1] CRAN (R 4.0.0)                      
##  stringi       1.5.3   2020-09-09 [1] CRAN (R 4.0.2)                      
##  stringr     * 1.4.0   2019-02-10 [1] CRAN (R 4.0.0)                      
##  testthat      3.0.1   2020-12-17 [1] CRAN (R 4.0.2)                      
##  tibble      * 3.0.4   2020-10-12 [1] CRAN (R 4.0.2)                      
##  tidyr       * 1.1.2   2020-08-27 [1] CRAN (R 4.0.2)                      
##  tidyselect    1.1.0   2020-05-11 [1] CRAN (R 4.0.0)                      
##  tidyverse   * 1.3.0   2019-11-21 [1] CRAN (R 4.0.0)                      
##  usethis       2.0.0   2020-12-10 [1] CRAN (R 4.0.2)                      
##  vctrs         0.3.6   2020-12-17 [1] CRAN (R 4.0.2)                      
##  withr         2.3.0   2020-09-22 [1] CRAN (R 4.0.2)                      
##  xfun          0.20    2021-01-06 [1] CRAN (R 4.0.2)                      
##  xml2          1.3.2   2020-04-23 [1] CRAN (R 4.0.0)                      
##  yaml          2.2.1   2020-02-01 [1] CRAN (R 4.0.0)                      
## 
## [1] /Library/Frameworks/R.framework/Versions/4.0/Resources/library
```
