---
title: "The grammar of graphics"
date: 2019-03-01

type: docs
toc: true
draft: false
aliases: ["/dataviz_grammar-of-graphics.html"]
categories: ["dataviz"]

menu:
  notes:
    parent: Data visualization
    weight: 2
---



{{% callout note %}}

This page is a summary of [*A Layered Grammar of Graphics*](http://www-tandfonline-com.proxy.uchicago.edu/doi/abs/10.1198/jcgs.2009.07098) by Hadley Wickham. I strongly encourage you to read the original article in conjunction with this summary.

{{% /callout %}}


```r
library(tidyverse)
library(knitr)
```

Google defines a **grammar** as "the whole system and structure of a language or of languages in general, usually taken as consisting of syntax and morphology (including inflections) and sometimes also phonology and semantics".^[[Google](https://www.google.com/search?q=grammar)] Others consider a grammar to be "the fundamental principles or rules of an art or science".^[[Wickham, Hadley. (2010) "A Layered Grammar of Graphics". *Journal of Computational and Graphical Statistics*, 19(1).](http://www.jstor.org.proxy.uchicago.edu/stable/25651297)] Applied to visualizations, a **grammar of graphics** is a grammar used to describe and create a wide range of statistical graphics.^[[Wilkinson, Leland. (2005). *The Grammar of Graphics*. (UChicago authentication required)](http://link.springer.com.proxy.uchicago.edu/book/10.1007%2F0-387-28695-0)]

The **layered grammar of graphics** approach is implemented in [`ggplot2`](https://cran.r-project.org/web/packages/ggplot2/index.html), a widely used graphics library for R. All graphics in this library are built using a layered approach, building layers up to create the final graphic.

## Components of the layered grammar of graphics

* Layer
    * Data
    * Mapping
    * Statistical transformation (stat)
    * Geometric object (geom)
    * Position adjustment (position)
* Scale
* Coordinate system (coord)
* Faceting (facet)
* Defaults
    * Data
    * Mapping

## Layer

**Layers** are used to create the objects on a plot. They are defined by five basic parts:

1. Data
1. Mapping
1. Statistical transformation (stat)
1. Geometric object (geom)
1. Position adjustment (position)

Layers are typically related to one another and share many common features. For instance, multiple layers can be built using the same underlying data. An example would be a scatterplot overlayed with a smoothed regression line to summarize the relationship between two variables:


```
## `geom_smooth()` using formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/layers-1.png" width="672" />

## Data and mapping

**Data** defines the source of the information to be visualized, but is independent from the other elements. So a layered graphic can be built which utilizes different data sources while keeping the other components the same.

For our running example, let's use the `mpg` dataset in the `ggplot2` package.^[Run `?mpg` in the console to get more information about this dataset.]


```r
head(x = mpg) %>%
  kable(caption = "Dataset of automobiles")
```



Table: Table 1: Dataset of automobiles

|manufacturer |model | displ| year| cyl|trans      |drv | cty| hwy|fl |class   |
|:------------|:-----|-----:|----:|---:|:----------|:---|---:|---:|:--|:-------|
|audi         |a4    |   1.8| 1999|   4|auto(l5)   |f   |  18|  29|p  |compact |
|audi         |a4    |   1.8| 1999|   4|manual(m5) |f   |  21|  29|p  |compact |
|audi         |a4    |   2.0| 2008|   4|manual(m6) |f   |  20|  31|p  |compact |
|audi         |a4    |   2.0| 2008|   4|auto(av)   |f   |  21|  30|p  |compact |
|audi         |a4    |   2.8| 1999|   6|auto(l5)   |f   |  16|  26|p  |compact |
|audi         |a4    |   2.8| 1999|   6|manual(m5) |f   |  18|  26|p  |compact |

**Mapping** defines how the variables are applied to the plot. So if we were graphing information from `mpg`, we might map a car's engine displacement to the $x$ position and highway mileage to the $y$ position.


```r
mpg %>%
  select(displ, hwy) %>%
  rename(x = displ,
         y = hwy)
```

```
## # A tibble: 234 x 2
##        x     y
##    <dbl> <int>
##  1   1.8    29
##  2   1.8    29
##  3   2      31
##  4   2      30
##  5   2.8    26
##  6   2.8    26
##  7   3.1    27
##  8   1.8    26
##  9   1.8    25
## 10   2      28
## # … with 224 more rows
```

## Statistical transformation

A **statistical transformation** (*stat*) transforms the data, generally by summarizing the information. For instance, in a bar graph you typically are not trying to graph the raw data because this doesn't make any inherent sense. Instead, you might summarize the data by graphing the total number of observations within a set of categories. Or if you have a dataset with many observations, you might transform the data into a smoothing line which summarizes the overall pattern of the relationship between variables by calculating the mean of $y$ conditional on $x$.

A stat is a function that takes in a dataset as the input and returns a dataset as the output; a stat can add new variables to the original dataset, or create an entirely new dataset. So instead of graphing this data in its raw form:


```r
mpg %>%
  select(cyl)
```

```
## # A tibble: 234 x 1
##      cyl
##    <int>
##  1     4
##  2     4
##  3     4
##  4     4
##  5     6
##  6     6
##  7     6
##  8     4
##  9     4
## 10     4
## # … with 224 more rows
```

You would transform it to:


```r
mpg %>%
  count(cyl)
```

```
## # A tibble: 4 x 2
##     cyl     n
##   <int> <int>
## 1     4    81
## 2     5     4
## 3     6    79
## 4     8    70
```

{{% callout note %}}

Sometimes you don't need to make a statistical transformation. For example, in a scatterplot you use the raw values for the $x$ and $y$ variables to map onto the graph. In these situations, the statistical transformation is an *identity* transformation - the stat simply passes in the original dataset and exports the exact same dataset.

{{% /callout %}}

## Geometric objects

**Geometric objects** (*geoms*) control the type of plot you create. Geoms are classified by their dimensionality:

* 0 dimensions - point, text
* 1 dimension - path, line
* 2 dimensions - polygon, interval

Each geom can only display certain **aesthetics** or visual attributes of the geom. For example, a point geom has position, color, shape, and size aesthetics.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = class)) +
  geom_point() +
  ggtitle("A point geom with position and color aesthetics")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/geom_point-1.png" width="672" />

* Position defines where each point is drawn on the plot
* Color defines the color of each point. Here the color is determined by the class of the car (observation)

Whereas a bar geom has position, height, width, and fill color.


```r
ggplot(data = mpg, aes(x = cyl)) +
  geom_bar() +
  ggtitle("A bar geom with position and height aesthetics")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/geom_bar-1.png" width="672" />

* Position determines the starting location (origin) of each bar
* Height determines how tall to draw the bar. Here the height is based on the number of observations in the dataset for each possible number of cylinders.

## Position adjustment

Sometimes with dense data we need to adjust the position of elements on the plot, otherwise data points might obscure one another. Bar plots frequently **stack** or **dodge** the bars to avoid overlap:


```r
count(x = mpg, class, cyl) %>%
  ggplot(mapping = aes(x = cyl, y = n, fill = class)) +
  geom_bar(stat = "identity") +
  ggtitle(label = "A stacked bar chart")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/position_dodge-1.png" width="672" />

```r
count(x = mpg, class, cyl) %>%
  ggplot(mapping = aes(x = cyl, y = n, fill = class)) +
  geom_bar(stat = "identity", position = "dodge") +
  ggtitle(label = "A dodged bar chart")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/position_dodge-2.png" width="672" />

Sometimes scatterplots with few unique $x$ and $y$ values are **jittered** (random noise is added) to reduce overplotting.


```r
ggplot(data = mpg, mapping = aes(x = cyl, y = hwy)) +
  geom_point() +
  ggtitle("A point geom with obscured data points")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/position-1.png" width="672" />

```r
ggplot(data = mpg, mapping = aes(x = cyl, y = hwy)) +
  geom_jitter() +
  ggtitle("A point geom with jittered data points")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/position-2.png" width="672" />

## Scale

A **scale** controls how data is mapped to aesthetic attributes, so we need one scale for every aesthetic property employed in a layer. For example, this graph defines a scale for color:


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = class)) +
  geom_point() +
  guides(color = guide_legend(override.aes = list(size = 4)))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/scale_color-1.png" width="672" />

Note that the scale is consistent - every point for a compact car is drawn in tan, whereas SUVs are drawn in pink. The scale can be changed to use a different color palette:


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = class)) +
  geom_point() +
  guides(color = guide_legend(override.aes = list(size = 4))) +
  scale_color_brewer(palette = "Dark2")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/scale_color_palette-1.png" width="672" />

Now we are using a different palette, but the scale is still consistent: all compact cars utilize the same color, whereas SUVs use a new color **but each SUV still uses the same, consistent color**.

## Coordinate system

A **coordinate system** (*coord*) maps the position of objects onto the plane of the plot, and controls how the axes and grid lines are drawn. Plots typically use two coordinates ($x, y$), but could use any number of coordinates. Most plots are drawn using the [**Cartesian coordinate system**](https://en.wikipedia.org/wiki/Cartesian_coordinate_system):


```r
x1 <- c(1, 10)
y1 <- c(1, 5)
p <- qplot(x = x1, y = y1, geom = "blank", xlab = NULL, ylab = NULL) +
  theme_bw()
p +
  ggtitle(label = "Cartesian coordinate system")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/coord_cart-1.png" width="672" />

This system requires a fixed and equal spacing between values on the axes. That is, the graph draws the same distance between 1 and 2 as it does between 5 and 6. The graph could be drawn using a [**semi-log coordinate system**](https://en.wikipedia.org/wiki/Semi-log_plot) which logarithmically compresses the distance on an axis:


```r
p +
  coord_trans(y = "log10") +
  ggtitle(label = "Semi-log coordinate system")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/coord_semi_log-1.png" width="672" />

Or could even be drawn using [**polar coordinates**](https://en.wikipedia.org/wiki/Polar_coordinate_system):


```r
p +
  coord_polar() +
  ggtitle(label = "Polar coordinate system")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/coord_polar-1.png" width="672" />

## Faceting

**Faceting** can be used to split the data up into subsets of the entire dataset. This is a powerful tool when investigating whether patterns are the same or different across conditions, and allows the subsets to be visualized on the same plot (known as **conditioned** or **trellis** plots). The faceting specification describes which variables should be used to split up the data, and how they should be arranged.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point() +
  facet_wrap(~ class)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/facet-1.png" width="672" />

## Defaults

Rather than explicitly declaring each component of a layered graphic (which will use more code and introduces opportunities for errors), we can establish intelligent defaults for specific geoms and scales. For instance, whenever we want to use a bar geom, we can default to using a stat that counts the number of observations in each group of our variable in the $x$ position.

Consider the following scenario: you wish to generate a scatterplot visualizing the relationship between engine displacement size and highway fuel efficiency. With no defaults, the code to generate this graph is:


```r
ggplot() +
  layer(
    data = mpg, mapping = aes(x = displ, y = hwy),
    geom = "point", stat = "identity", position = "identity"
  ) +
  scale_x_continuous() +
  scale_y_continuous() +
  coord_cartesian()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/default-1.png" width="672" />

The above code:

* Creates a new plot object (`ggplot`)
* Adds a layer (`layer`)
    * Specifies the data (`mpg`)
    * Maps engine displacement to the $x$ position and highway mileage to the $y$ position (`mapping`)
    * Uses the point geometric transformation (`geom = "point"`)
    * Implements an identity transformation and position (`stat = "identity"` and `position = "identity"`)
* Establishes two continuous position scales (`scale_x_continuous` and `scale_y_continuous`)
* Declares a cartesian coordinate system (`coord_cartesian`)

How can we simplify this using intelligent defaults?

1. We only need to specify one geom and stat, since each geom has a default stat.
1. Cartesian coordinate systems are most commonly used, so it should be the default.
1. Default scales can be added based on the aesthetic and type of variables.
    * Continuous values are transformed with a linear scaling.
    * Discrete values are mapped to integers.
    * Scales for aesthetics such as color, fill, and size can also be intelligently defaulted.

Using these defaults, we can rewrite the above code as:


```r
ggplot() +
  geom_point(data = mpg, mapping = aes(x = displ, y = hwy))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/default2-1.png" width="672" />

This generates the exact same plot, but uses fewer lines of code. Because multiple layers can use the same components (data, mapping, etc.), we can also specify that information in the `ggplot()` function rather than in the `layer()` function:


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/default3-1.png" width="672" />

And as we will learn, function arguments in R use specific ordering, so we can omit the explicit call to `data` and `mapping`:


```r
ggplot(mpg, aes(displ, hwy)) +
  geom_point()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/default4-1.png" width="672" />

With this specification, it is easy to build the graphic up with additional layers, without modifying the original code:


```r
ggplot(mpg, aes(displ, hwy)) +
  geom_point() +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/default5-1.png" width="672" />

Because we called `aes(displ, hwy)` within the `ggplot()` function, it is automatically passed along to both `geom_point()` and `geom_smooth()`. If we fail to do this, we get an error:


```r
ggplot(mpg) +
  geom_point(aes(displ, hwy)) +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

```
## Error: stat_smooth requires the following missing aesthetics: x and y
```

<img src="{{< blogdown/postref >}}index_files/figure-html/default6-1.png" width="672" />

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
##  here          1.0.1   2020-12-13 [1] CRAN (R 4.0.2)                      
##  hms           0.5.3   2020-01-08 [1] CRAN (R 4.0.0)                      
##  htmltools     0.5.1   2021-01-12 [1] CRAN (R 4.0.2)                      
##  httr          1.4.2   2020-07-20 [1] CRAN (R 4.0.2)                      
##  jsonlite      1.7.2   2020-12-09 [1] CRAN (R 4.0.2)                      
##  knitr       * 1.30    2020-09-22 [1] CRAN (R 4.0.2)                      
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
