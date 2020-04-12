---
title: "Practice using relational data"
date: 2019-03-01

type: docs
toc: true
draft: false
aliases: ["/datawrangle_relational_data_exercise.html"]
categories: ["datawrangle"]

menu:
  notes:
    parent: Data wrangling
    weight: 5
---




```r
library(tidyverse)
library(nycflights13)
theme_set(theme_minimal())
```

For each exercise, use your knowledge of relational data and joining operations to compute a table or graph that answers the question. All questions use data frames from the `nycflights13` package (if you have not previously installed it, do so using `install.packages("nycflights13")`).

{{% alert note %}}

[Review the database structure before you begin the exercises.](http://r4ds.had.co.nz/relational-data.html#nycflights13-relational)

{{% /alert %}}

## Is there a relationship between the age of a plane and its departure delays?

Hint: all the data is from 2013.

<details> 
  <summary>Click for the solution</summary>
  <p>
  
The first step is to calculate the age of each plane. To do that, use `planes` and the `age` variable:


```r
(plane_ages <- planes %>%
  mutate(age = 2013 - year) %>%
  select(tailnum, age))
```

```
## # A tibble: 3,322 x 2
##    tailnum   age
##    <chr>   <dbl>
##  1 N10156      9
##  2 N102UW     15
##  3 N103US     14
##  4 N104UW     14
##  5 N10575     11
##  6 N105UW     14
##  7 N107US     14
##  8 N108UW     14
##  9 N109UW     14
## 10 N110UW     14
## # … with 3,312 more rows
```

The best approach to answering this question is a visualization. There are several different types of visualizations you could implement (e.g. scatterplot with smoothing line, line graph of average delay by age). The important thing is that we need to combine `flights` with `plane_ages` to determine for each flight the age of the plane. This is another mutating join. The best choice is `inner_join()` as this will automatically remove any rows in `flights` where we don't have age data on the plane.


```r
# smoothing line
flights %>%
  inner_join(y = plane_ages) %>%
  ggplot(mapping = aes(x = age, y = dep_delay)) +
  geom_smooth()
```

```
## Joining, by = "tailnum"
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

```
## Warning: Removed 9374 rows containing non-finite values (stat_smooth).
```

<img src="/notes/relational-data-exercise_files/figure-html/age-delay-solution-1.png" width="672" />

```r
# line graph of average delay by age
flights %>%
  inner_join(y = plane_ages) %>%
  group_by(age) %>%
  summarise(delay = mean(dep_delay, na.rm = TRUE)) %>%
  ggplot(mapping = aes(x = age, y = delay)) +
  geom_point() +
  geom_line()
```

```
## Joining, by = "tailnum"
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

```
## Warning: Removed 1 rows containing missing values (geom_path).
```

<img src="/notes/relational-data-exercise_files/figure-html/age-delay-solution-2.png" width="672" />

In this situation, `left_join()` could also be used because `ggplot()` and `mean(na.rm = TRUE)` drop missing values (remember that `left_join()` keeps all rows from `flights`, even if we don't have information on the plane).


```r
flights %>%
  left_join(y = plane_ages) %>%
  ggplot(mapping = aes(x = age, y = dep_delay)) +
  geom_smooth()
```

```
## Joining, by = "tailnum"
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

```
## Warning: Removed 61980 rows containing non-finite values (stat_smooth).
```

<img src="/notes/relational-data-exercise_files/figure-html/age-delay-leftjoin-1.png" width="672" />

```r
flights %>%
  left_join(y = plane_ages) %>%
  group_by(age) %>%
  summarise(delay = mean(dep_delay, na.rm = TRUE)) %>%
  ggplot(mapping = aes(x = age, y = delay)) +
  geom_point() +
  geom_line()
```

```
## Joining, by = "tailnum"
```

```
## Warning: Removed 2 rows containing missing values (geom_point).
```

```
## Warning: Removed 2 rows containing missing values (geom_path).
```

<img src="/notes/relational-data-exercise_files/figure-html/age-delay-leftjoin-2.png" width="672" />

The important takeaway is that departure delays do not appear to increase with plane age -- in fact they seem to decrease slightly (though with an expanding confidence interval). Care to think of a reason why this may be so?

  </p>
</details>

## Add the location of the origin and destination (i.e. the `lat` and `lon`) to `flights`.

<details> 
  <summary>Click for the solution</summary>
  <p>
  
This is a mutating join, and the basic function you need to use here is `left_join()`. We have to perform the joining operation twice since we want to create new variables based on both the destination airport and the origin airport. And because the name of the key variable differs between the data frames, we need to explicitly define how to join the data frames using the `by` argument:


```r
flights %>%
  left_join(y = airports, by = c(dest = "faa")) %>%
  left_join(y = airports, by = c(origin = "faa"))
```

```
## # A tibble: 336,776 x 33
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
##  1  2013     1     1      517            515         2      830
##  2  2013     1     1      533            529         4      850
##  3  2013     1     1      542            540         2      923
##  4  2013     1     1      544            545        -1     1004
##  5  2013     1     1      554            600        -6      812
##  6  2013     1     1      554            558        -4      740
##  7  2013     1     1      555            600        -5      913
##  8  2013     1     1      557            600        -3      709
##  9  2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # … with 336,766 more rows, and 26 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>, name.x <chr>, lat.x <dbl>,
## #   lon.x <dbl>, alt.x <int>, tz.x <dbl>, dst.x <chr>, tzone.x <chr>,
## #   name.y <chr>, lat.y <dbl>, lon.y <dbl>, alt.y <int>, tz.y <dbl>,
## #   dst.y <chr>, tzone.y <chr>
```

Notice that with this approach, we are joining **all** of the columns in `airports`. The instructions just asked for latitude and longitude, so we can create a copy of `airports` that only includes the necessary variables (`lat` and `lon`, plus the primary key variable `faa`) and join `flights` to that data frame:


```r
airports_lite <- airports %>%
  select(faa, lat, lon)

flights %>%
  left_join(y = airports_lite, by = c(dest = "faa")) %>%
  left_join(y = airports_lite, by = c(origin = "faa"))
```

```
## # A tibble: 336,776 x 23
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
##  1  2013     1     1      517            515         2      830
##  2  2013     1     1      533            529         4      850
##  3  2013     1     1      542            540         2      923
##  4  2013     1     1      544            545        -1     1004
##  5  2013     1     1      554            600        -6      812
##  6  2013     1     1      554            558        -4      740
##  7  2013     1     1      555            600        -5      913
##  8  2013     1     1      557            600        -3      709
##  9  2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # … with 336,766 more rows, and 16 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>, lat.x <dbl>, lon.x <dbl>, lat.y <dbl>,
## #   lon.y <dbl>
```

This is better, but now we have two sets of latitude and longitude variables in the data frame: one for the destination airport, and one for the origin airport. When we perform the second `left_join()` operation, to avoid duplicate variable names the function automatically adds generic `.x` and `.y` suffixes to the output to disambiguate them. This is nice, but we might want something more intuitive to explicitly identify which variables are associated with the destination vs. the origin. To do that, we override the default `suffix` argument with custom suffixes:


```r
airports_lite <- airports %>%
  select(faa, lat, lon)

flights %>%
  left_join(y = airports_lite, by = c(dest = "faa")) %>%
  left_join(y = airports_lite, by = c(origin = "faa"), suffix = c(".dest", ".origin"))
```

```
## # A tibble: 336,776 x 23
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
##  1  2013     1     1      517            515         2      830
##  2  2013     1     1      533            529         4      850
##  3  2013     1     1      542            540         2      923
##  4  2013     1     1      544            545        -1     1004
##  5  2013     1     1      554            600        -6      812
##  6  2013     1     1      554            558        -4      740
##  7  2013     1     1      555            600        -5      913
##  8  2013     1     1      557            600        -3      709
##  9  2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # … with 336,766 more rows, and 16 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>, lat.dest <dbl>, lon.dest <dbl>,
## #   lat.origin <dbl>, lon.origin <dbl>
```

  </p>
</details>

### Acknowledgements

* Exercises drawn from [**Relational Data** in *R for Data Science*](http://r4ds.had.co.nz/relational-data.html)

## Session Info



```r
devtools::session_info()
```

```
## ─ Session info ───────────────────────────────────────────────────────────────
##  setting  value                       
##  version  R version 3.6.3 (2020-02-29)
##  os       macOS Catalina 10.15.4      
##  system   x86_64, darwin15.6.0        
##  ui       X11                         
##  language (EN)                        
##  collate  en_US.UTF-8                 
##  ctype    en_US.UTF-8                 
##  tz       America/Chicago             
##  date     2020-04-10                  
## 
## ─ Packages ───────────────────────────────────────────────────────────────────
##  package      * version     date       lib source                      
##  assertthat     0.2.1       2019-03-21 [1] CRAN (R 3.6.0)              
##  backports      1.1.5       2019-10-02 [1] CRAN (R 3.6.0)              
##  blogdown       0.18        2020-03-04 [1] CRAN (R 3.6.0)              
##  bookdown       0.18        2020-03-05 [1] CRAN (R 3.6.0)              
##  broom          0.5.5       2020-02-29 [1] CRAN (R 3.6.0)              
##  callr          3.4.2       2020-02-12 [1] CRAN (R 3.6.1)              
##  cellranger     1.1.0       2016-07-27 [1] CRAN (R 3.6.0)              
##  cli            2.0.2       2020-02-28 [1] CRAN (R 3.6.0)              
##  colorspace     1.4-1       2019-03-18 [1] CRAN (R 3.6.0)              
##  crayon         1.3.4       2017-09-16 [1] CRAN (R 3.6.0)              
##  DBI            1.1.0       2019-12-15 [1] CRAN (R 3.6.0)              
##  dbplyr         1.4.2       2019-06-17 [1] CRAN (R 3.6.0)              
##  desc           1.2.0       2018-05-01 [1] CRAN (R 3.6.0)              
##  devtools       2.2.2       2020-02-17 [1] CRAN (R 3.6.0)              
##  digest         0.6.25      2020-02-23 [1] CRAN (R 3.6.0)              
##  dplyr        * 0.8.5       2020-03-07 [1] CRAN (R 3.6.0)              
##  ellipsis       0.3.0       2019-09-20 [1] CRAN (R 3.6.0)              
##  evaluate       0.14        2019-05-28 [1] CRAN (R 3.6.0)              
##  fansi          0.4.1       2020-01-08 [1] CRAN (R 3.6.0)              
##  forcats      * 0.5.0       2020-03-01 [1] CRAN (R 3.6.0)              
##  fs             1.3.2       2020-03-05 [1] CRAN (R 3.6.0)              
##  generics       0.0.2       2018-11-29 [1] CRAN (R 3.6.0)              
##  ggplot2      * 3.3.0       2020-03-05 [1] CRAN (R 3.6.0)              
##  glue           1.3.2       2020-03-12 [1] CRAN (R 3.6.0)              
##  gtable         0.3.0       2019-03-25 [1] CRAN (R 3.6.0)              
##  haven          2.2.0       2019-11-08 [1] CRAN (R 3.6.0)              
##  here           0.1         2017-05-28 [1] CRAN (R 3.6.0)              
##  hms            0.5.3       2020-01-08 [1] CRAN (R 3.6.0)              
##  htmltools      0.4.0       2019-10-04 [1] CRAN (R 3.6.0)              
##  httr           1.4.1       2019-08-05 [1] CRAN (R 3.6.0)              
##  jsonlite       1.6.1       2020-02-02 [1] CRAN (R 3.6.0)              
##  knitr          1.28        2020-02-06 [1] CRAN (R 3.6.0)              
##  lattice        0.20-40     2020-02-19 [1] CRAN (R 3.6.0)              
##  lifecycle      0.2.0       2020-03-06 [1] CRAN (R 3.6.0)              
##  lubridate      1.7.4       2018-04-11 [1] CRAN (R 3.6.0)              
##  magrittr       1.5         2014-11-22 [1] CRAN (R 3.6.0)              
##  memoise        1.1.0       2017-04-21 [1] CRAN (R 3.6.0)              
##  modelr         0.1.6       2020-02-22 [1] CRAN (R 3.6.0)              
##  munsell        0.5.0       2018-06-12 [1] CRAN (R 3.6.0)              
##  nlme           3.1-145     2020-03-04 [1] CRAN (R 3.6.0)              
##  nycflights13 * 1.0.1       2019-09-16 [1] CRAN (R 3.6.0)              
##  pillar         1.4.3       2019-12-20 [1] CRAN (R 3.6.0)              
##  pkgbuild       1.0.6       2019-10-09 [1] CRAN (R 3.6.0)              
##  pkgconfig      2.0.3       2019-09-22 [1] CRAN (R 3.6.0)              
##  pkgload        1.0.2       2018-10-29 [1] CRAN (R 3.6.0)              
##  prettyunits    1.1.1       2020-01-24 [1] CRAN (R 3.6.0)              
##  processx       3.4.2       2020-02-09 [1] CRAN (R 3.6.0)              
##  ps             1.3.2       2020-02-13 [1] CRAN (R 3.6.0)              
##  purrr        * 0.3.3       2019-10-18 [1] CRAN (R 3.6.0)              
##  R6             2.4.1       2019-11-12 [1] CRAN (R 3.6.0)              
##  Rcpp           1.0.4       2020-03-17 [1] CRAN (R 3.6.0)              
##  readr        * 1.3.1       2018-12-21 [1] CRAN (R 3.6.0)              
##  readxl         1.3.1       2019-03-13 [1] CRAN (R 3.6.0)              
##  remotes        2.1.1       2020-02-15 [1] CRAN (R 3.6.0)              
##  reprex         0.3.0       2019-05-16 [1] CRAN (R 3.6.0)              
##  rlang          0.4.5.9000  2020-03-19 [1] Github (r-lib/rlang@a90b04b)
##  rmarkdown      2.1         2020-01-20 [1] CRAN (R 3.6.0)              
##  rprojroot      1.3-2       2018-01-03 [1] CRAN (R 3.6.0)              
##  rstudioapi     0.11        2020-02-07 [1] CRAN (R 3.6.0)              
##  rvest          0.3.5       2019-11-08 [1] CRAN (R 3.6.0)              
##  scales         1.1.0       2019-11-18 [1] CRAN (R 3.6.0)              
##  sessioninfo    1.1.1       2018-11-05 [1] CRAN (R 3.6.0)              
##  stringi        1.4.6       2020-02-17 [1] CRAN (R 3.6.0)              
##  stringr      * 1.4.0       2019-02-10 [1] CRAN (R 3.6.0)              
##  testthat       2.3.2       2020-03-02 [1] CRAN (R 3.6.0)              
##  tibble       * 2.1.3       2019-06-06 [1] CRAN (R 3.6.0)              
##  tidyr        * 1.0.2       2020-01-24 [1] CRAN (R 3.6.0)              
##  tidyselect     1.0.0       2020-01-27 [1] CRAN (R 3.6.0)              
##  tidyverse    * 1.3.0       2019-11-21 [1] CRAN (R 3.6.0)              
##  usethis        1.5.1       2019-07-04 [1] CRAN (R 3.6.0)              
##  vctrs          0.2.99.9010 2020-03-19 [1] Github (r-lib/vctrs@94bea91)
##  withr          2.1.2       2018-03-15 [1] CRAN (R 3.6.0)              
##  xfun           0.12        2020-01-13 [1] CRAN (R 3.6.0)              
##  xml2           1.2.5       2020-03-11 [1] CRAN (R 3.6.0)              
##  yaml           2.2.1       2020-02-01 [1] CRAN (R 3.6.0)              
## 
## [1] /Library/Frameworks/R.framework/Versions/3.6/Resources/library
```
