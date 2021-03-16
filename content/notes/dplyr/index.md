---
title: "dplyr in brief"
date: 2019-03-01

type: docs
toc: true
draft: false
aliases: ["/datawrangle_dplyr.html"]
categories: ["datawrangle"]

menu:
  notes:
    parent: Data wrangling
    weight: 2
---




```r
library(tidyverse)
library(nycflights13)
```

![Data science workflow](/img/data-science.png)

Rarely will your data arrive in exactly the form you require in order to analyze it appropriately. As part of the data science workflow you will need to **transform** your data in order to analyze it. Just as we established a syntax for generating graphics (the **layered grammar of graphics**), so too will we have a syntax for data transformation.

From the same author of `ggplot2`, I give you `dplyr`! This package contains useful functions for transforming and manipulating data frames, the bread-and-butter format for data in R. These functions can be thought of as **verbs**. The noun is the data, and the verb is acting on the noun. All of the `dplyr` verbs (and in fact all the verbs in the wider `tidyverse`) work similarly:

1. The first argument is a data frame
1. Subsequent arguments describe what to do with the data frame
1. The result is a new data frame

![Artwork by @allison_horst](/img/allison_horst_art/dplyr_wrangling.png) 

## Key functions in `dplyr`

`function()`    | Action performed
----------------|--------------------------------------------------------
`filter()`      | Subsets observations based on their values
`arrange()`     | Changes the order of observations based on their values
`select()`      | Selects a subset of columns from the data frame
`rename()`      | Changes the name of columns in the data frame
`mutate()`      | Creates new columns (or variables)
`group_by()`    | Changes the unit of analysis from the complete dataset to individual groups
`summarize()`   | Collapses the data frame to a smaller number of rows which summarize the larger data

![Artwork by @allison_horst](/img/allison_horst_art/dplyr_mutate.png)

These are the basic verbs you will use to transform your data. By combining them together, you can perform powerful data manipulation tasks.

## American vs. British English

Hadley Wickham is from New Zealand. As such he (and base R) favours British spellings:

{{< tweet 405707093770244097 >}}

While British spelling is perhaps the norm, this is America!

![](https://media.giphy.com/media/8KnfG3knLExpu/giphy.gif)<!-- -->

Fortunately many R functions can be written using American or British variants:

* `summarize()` = `summarise()`
* `color()` = `colour()`

Therefore in this class I will generally stick to American spelling.

## Saving transformed data

`dplyr` never overwrites existing data. If you want a copy of the transformed data for later use in the program, you need to explicitly save it. You can do this by using the assignment operator `<-`:


```r
filter(.data = diamonds, cut == "Ideal")  # printed, but not saved
```

```
## # A tibble: 21,551 x 10
##    carat cut   color clarity depth table price     x     y     z
##    <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43
##  2  0.23 Ideal J     VS1      62.8    56   340  3.93  3.9   2.46
##  3  0.31 Ideal J     SI2      62.2    54   344  4.35  4.37  2.71
##  4  0.3  Ideal I     SI2      62      54   348  4.31  4.34  2.68
##  5  0.33 Ideal I     SI2      61.8    55   403  4.49  4.51  2.78
##  6  0.33 Ideal I     SI2      61.2    56   403  4.49  4.5   2.75
##  7  0.33 Ideal J     SI1      61.1    56   403  4.49  4.55  2.76
##  8  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
##  9  0.32 Ideal I     SI1      60.9    55   404  4.45  4.48  2.72
## 10  0.3  Ideal I     SI2      61      59   405  4.3   4.33  2.63
## # … with 21,541 more rows
```

```r
diamonds_ideal <- filter(.data = diamonds, cut == "Ideal")  # saved, but not printed
(diamonds_ideal <- filter(.data = diamonds, cut == "Ideal"))  # saved and printed
```

```
## # A tibble: 21,551 x 10
##    carat cut   color clarity depth table price     x     y     z
##    <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43
##  2  0.23 Ideal J     VS1      62.8    56   340  3.93  3.9   2.46
##  3  0.31 Ideal J     SI2      62.2    54   344  4.35  4.37  2.71
##  4  0.3  Ideal I     SI2      62      54   348  4.31  4.34  2.68
##  5  0.33 Ideal I     SI2      61.8    55   403  4.49  4.51  2.78
##  6  0.33 Ideal I     SI2      61.2    56   403  4.49  4.5   2.75
##  7  0.33 Ideal J     SI1      61.1    56   403  4.49  4.55  2.76
##  8  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
##  9  0.32 Ideal I     SI1      60.9    55   404  4.45  4.48  2.72
## 10  0.3  Ideal I     SI2      61      59   405  4.3   4.33  2.63
## # … with 21,541 more rows
```

![Artwork by @allison_horst](/img/allison_horst_art/dplyr_filter.jpg)

{{% callout note %}}

Do not use `=` to assign objects. [Read this for more information on the difference between `<-` and `=`.](http://stackoverflow.com/a/1742550)

{{% /callout %}}

## Using backticks to refer to column names

Normally within `tidyverse` functions you can refer to column names directly. For example,


```r
count(x = diamonds, color)
```

```
## # A tibble: 7 x 2
##   color     n
##   <ord> <int>
## 1 D      6775
## 2 E      9797
## 3 F      9542
## 4 G     11292
## 5 H      8304
## 6 I      5422
## 7 J      2808
```

`color` is a column in `diamonds` so I can refer to it directly within `count()`. However this becomes a problem for any column name that is **non-syntactic**.^[See [*Advanced R*](https://adv-r.hadley.nz/names-values.html#non-syntactic) for a more detailed discussion - but note that the book is called * **Advanced** R* for a reason.] A **syntactic** name consists only of letters, digits, and `.` and `_`. Examples of non-syntactic column names include:

* `Social conservative`
* `7-point ideology`
* `_id`

Any time you encounter a column that contains non-syntactic characters, you should refer to the column name using backticks ``` `` ```.


```r
count(x = diamonds, `color`)
```

```
## # A tibble: 7 x 2
##   color     n
##   <ord> <int>
## 1 D      6775
## 2 E      9797
## 3 F      9542
## 4 G     11292
## 5 H      8304
## 6 I      5422
## 7 J      2808
```

**Do not use quotation marks (`''` or `""`) to refer to the column name.** This appears to work, but is not consistent and will fail when you do not expect it. Consider the same operation as above but using quotation marks instead of backticks.


```r
count(x = diamonds, "color")
```

```
## # A tibble: 1 x 2
##   `"color"`     n
##   <chr>     <int>
## 1 color     53940
```

The word "color" has been duplicated 53940 times and tabulated using the `count()` function. Not what we intended. Always use the backticks for non-syntactic column names.

## Missing values

`NA` represents an unknown value. Missing values are contagious, in that their properties will transfer to any operation performed on it.


```r
NA > 5
```

```
## [1] NA
```

```r
10 == NA
```

```
## [1] NA
```

```r
NA + 10
```

```
## [1] NA
```

To determine if a value is missing, use the `is.na()` function.

When filtering, you must explicitly call for missing values to be returned.


```r
df <- tibble(x = c(1, NA, 3))
df
```

```
## # A tibble: 3 x 1
##       x
##   <dbl>
## 1     1
## 2    NA
## 3     3
```

```r
filter(df, x > 1)
```

```
## # A tibble: 1 x 1
##       x
##   <dbl>
## 1     3
```

```r
filter(df, is.na(x) | x > 1)
```

```
## # A tibble: 2 x 1
##       x
##   <dbl>
## 1    NA
## 2     3
```

Or when calculating summary statistics, you need to explicitly ignore missing values.


```r
df <- tibble(
  x = c(1, 2, 3, 5, NA)
)
df
```

```
## # A tibble: 5 x 1
##       x
##   <dbl>
## 1     1
## 2     2
## 3     3
## 4     5
## 5    NA
```

```r
summarize(df, meanx = mean(x))
```

```
## # A tibble: 1 x 1
##   meanx
##   <dbl>
## 1    NA
```

```r
summarize(df, meanx = mean(x, na.rm = TRUE))
```

```
## # A tibble: 1 x 1
##   meanx
##   <dbl>
## 1  2.75
```

## Piping

As we discussed, frequently you need to perform a series of intermediate steps to transform data for analysis. If we write each step as a discrete command and store their contents as new objects, your code can become convoluted.

Drawing on [this example from *R for Data Science*](http://r4ds.had.co.nz/transform.html#combining-multiple-operations-with-the-pipe), let's explore the relationship between the distance and average delay for each location. At this point, we would write it something like this:


```r
by_dest <- group_by(.data = flights, dest)
delay <- summarise(
  .data = by_dest,
  count = n(),
  dist = mean(distance, na.rm = TRUE),
  delay = mean(arr_delay, na.rm = TRUE)
)
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```r
delay <- filter(.data = delay, count > 20, dest != "HNL")

ggplot(data = delay, mapping = aes(x = dist, y = delay)) +
  geom_point(aes(size = count), alpha = 1/3) +
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/intermediate-1.png" width="672" />

Decomposing the problem, there are three basic steps:

1. Group `flights` by destination.
1. Summarize to compute distance, average delay, and number of flights.
1. Filter to remove noisy points and the Honolulu airport, which is almost twice as far away as the next closest airport.

The code as written is inefficient because we have to name and store each intermediate data frame, even though we don't care about them. It also provides more opportunities for typos and errors.

Because all `dplyr` verbs follow the same syntax (data first, then options for the function), we can use the pipe operator `%>%` to **chain** a series of functions together in one command:


```r
delays <- flights %>% 
  group_by(dest) %>% 
  summarize(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  filter(count > 20, dest != "HNL")
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

Now, we don't have to name each intermediate step and store them as data frames. We only store a single data frame (`delays`) which contains the final version of the transformed data frame. We could read this code as use the `flights` data, then group by destination, then summarize for each destination the number of flights, the average disance, and the average delay, then subset only the destinations with at least 20 flights and exclude Honolulu.

## Things to not do with piping

Remember that with pipes, we don't have to save all of our intermediate steps. We only use one assignment, like this:


```r
delays <- flights %>% 
  group_by(dest) %>% 
  summarize(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  filter(count > 20, dest != "HNL")
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

Do not do this:


```r
delays <- flights %>% 
  by_dest <- group_by(dest) %>% 
  delay <- summarize(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  delay <- filter(count > 20, dest != "HNL")
```

```
Error: bad assignment: 
     summarize(count = n(), dist = mean(distance, na.rm = TRUE), delay = mean(arr_delay, 
         na.rm = TRUE)) %>% delay <- filter(count > 20, dest != "HNL")
```

Or this:


```r
delays <- flights %>% 
  group_by(.data = flights, dest) %>% 
  summarize(.data = flights,
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  filter(.data = flights, count > 20, dest != "HNL")
```

```
## Error: Must group by variables found in `.data`.
## * Column `.` is not found.
```

If you use pipes, you don't have to reference the data frame with each function - just the first time at the beginning of the pipe sequence.

## Acknowledgments

* Artwork by [@allison_horst](https://github.com/allisonhorst/stats-illustrations)

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
##  date     2021-01-21                  
## 
## ─ Packages ───────────────────────────────────────────────────────────────────
##  package      * version date       lib source                              
##  assertthat     0.2.1   2019-03-21 [1] CRAN (R 4.0.0)                      
##  backports      1.2.1   2020-12-09 [1] CRAN (R 4.0.2)                      
##  blogdown       1.1     2021-01-19 [1] CRAN (R 4.0.3)                      
##  bookdown       0.21    2020-10-13 [1] CRAN (R 4.0.2)                      
##  broom          0.7.3   2020-12-16 [1] CRAN (R 4.0.2)                      
##  callr          3.5.1   2020-10-13 [1] CRAN (R 4.0.2)                      
##  cellranger     1.1.0   2016-07-27 [1] CRAN (R 4.0.0)                      
##  cli            2.2.0   2020-11-20 [1] CRAN (R 4.0.2)                      
##  colorspace     2.0-0   2020-11-11 [1] CRAN (R 4.0.2)                      
##  crayon         1.3.4   2017-09-16 [1] CRAN (R 4.0.0)                      
##  DBI            1.1.0   2019-12-15 [1] CRAN (R 4.0.0)                      
##  dbplyr         2.0.0   2020-11-03 [1] CRAN (R 4.0.2)                      
##  desc           1.2.0   2018-05-01 [1] CRAN (R 4.0.0)                      
##  devtools       2.3.2   2020-09-18 [1] CRAN (R 4.0.2)                      
##  digest         0.6.27  2020-10-24 [1] CRAN (R 4.0.2)                      
##  dplyr        * 1.0.2   2020-08-18 [1] CRAN (R 4.0.2)                      
##  ellipsis       0.3.1   2020-05-15 [1] CRAN (R 4.0.0)                      
##  evaluate       0.14    2019-05-28 [1] CRAN (R 4.0.0)                      
##  fansi          0.4.1   2020-01-08 [1] CRAN (R 4.0.0)                      
##  forcats      * 0.5.0   2020-03-01 [1] CRAN (R 4.0.0)                      
##  fs             1.5.0   2020-07-31 [1] CRAN (R 4.0.2)                      
##  generics       0.1.0   2020-10-31 [1] CRAN (R 4.0.2)                      
##  ggplot2      * 3.3.3   2020-12-30 [1] CRAN (R 4.0.2)                      
##  glue           1.4.2   2020-08-27 [1] CRAN (R 4.0.2)                      
##  gtable         0.3.0   2019-03-25 [1] CRAN (R 4.0.0)                      
##  haven          2.3.1   2020-06-01 [1] CRAN (R 4.0.0)                      
##  here           1.0.1   2020-12-13 [1] CRAN (R 4.0.2)                      
##  hms            0.5.3   2020-01-08 [1] CRAN (R 4.0.0)                      
##  htmltools      0.5.1   2021-01-12 [1] CRAN (R 4.0.2)                      
##  httr           1.4.2   2020-07-20 [1] CRAN (R 4.0.2)                      
##  jsonlite       1.7.2   2020-12-09 [1] CRAN (R 4.0.2)                      
##  knitr          1.30    2020-09-22 [1] CRAN (R 4.0.2)                      
##  lifecycle      0.2.0   2020-03-06 [1] CRAN (R 4.0.0)                      
##  lubridate      1.7.9.2 2021-01-18 [1] Github (tidyverse/lubridate@aab2e30)
##  magrittr       2.0.1   2020-11-17 [1] CRAN (R 4.0.2)                      
##  memoise        1.1.0   2017-04-21 [1] CRAN (R 4.0.0)                      
##  modelr         0.1.8   2020-05-19 [1] CRAN (R 4.0.0)                      
##  munsell        0.5.0   2018-06-12 [1] CRAN (R 4.0.0)                      
##  nycflights13 * 1.0.1   2019-09-16 [1] CRAN (R 4.0.0)                      
##  pillar         1.4.7   2020-11-20 [1] CRAN (R 4.0.2)                      
##  pkgbuild       1.2.0   2020-12-15 [1] CRAN (R 4.0.2)                      
##  pkgconfig      2.0.3   2019-09-22 [1] CRAN (R 4.0.0)                      
##  pkgload        1.1.0   2020-05-29 [1] CRAN (R 4.0.0)                      
##  prettyunits    1.1.1   2020-01-24 [1] CRAN (R 4.0.0)                      
##  processx       3.4.5   2020-11-30 [1] CRAN (R 4.0.2)                      
##  ps             1.5.0   2020-12-05 [1] CRAN (R 4.0.2)                      
##  purrr        * 0.3.4   2020-04-17 [1] CRAN (R 4.0.0)                      
##  R6             2.5.0   2020-10-28 [1] CRAN (R 4.0.2)                      
##  Rcpp           1.0.6   2021-01-15 [1] CRAN (R 4.0.2)                      
##  readr        * 1.4.0   2020-10-05 [1] CRAN (R 4.0.2)                      
##  readxl         1.3.1   2019-03-13 [1] CRAN (R 4.0.0)                      
##  remotes        2.2.0   2020-07-21 [1] CRAN (R 4.0.2)                      
##  reprex         0.3.0   2019-05-16 [1] CRAN (R 4.0.0)                      
##  rlang          0.4.10  2020-12-30 [1] CRAN (R 4.0.2)                      
##  rmarkdown      2.6     2020-12-14 [1] CRAN (R 4.0.2)                      
##  rprojroot      2.0.2   2020-11-15 [1] CRAN (R 4.0.2)                      
##  rstudioapi     0.13    2020-11-12 [1] CRAN (R 4.0.2)                      
##  rvest          0.3.6   2020-07-25 [1] CRAN (R 4.0.2)                      
##  scales         1.1.1   2020-05-11 [1] CRAN (R 4.0.0)                      
##  sessioninfo    1.1.1   2018-11-05 [1] CRAN (R 4.0.0)                      
##  stringi        1.5.3   2020-09-09 [1] CRAN (R 4.0.2)                      
##  stringr      * 1.4.0   2019-02-10 [1] CRAN (R 4.0.0)                      
##  testthat       3.0.1   2020-12-17 [1] CRAN (R 4.0.2)                      
##  tibble       * 3.0.4   2020-10-12 [1] CRAN (R 4.0.2)                      
##  tidyr        * 1.1.2   2020-08-27 [1] CRAN (R 4.0.2)                      
##  tidyselect     1.1.0   2020-05-11 [1] CRAN (R 4.0.0)                      
##  tidyverse    * 1.3.0   2019-11-21 [1] CRAN (R 4.0.0)                      
##  usethis        2.0.0   2020-12-10 [1] CRAN (R 4.0.2)                      
##  vctrs          0.3.6   2020-12-17 [1] CRAN (R 4.0.2)                      
##  withr          2.3.0   2020-09-22 [1] CRAN (R 4.0.2)                      
##  xfun           0.20    2021-01-06 [1] CRAN (R 4.0.2)                      
##  xml2           1.3.2   2020-04-23 [1] CRAN (R 4.0.0)                      
##  yaml           2.2.1   2020-02-01 [1] CRAN (R 4.0.0)                      
## 
## [1] /Library/Frameworks/R.framework/Versions/4.0/Resources/library
```
