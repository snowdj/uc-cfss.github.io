---
title: "The basics of statistical learning"
date: 2019-03-01

type: docs
toc: true
draft: false
aliases: ["/stat001_statistical_learning.html"]
categories: ["stat-learn"]

menu:
  notes:
    parent: Statistical learning
    weight: 1
---





**Statistical models** attempt to summarize relationships between variables by reducing the dimensionality of the data. For example, here we have some simulated data on sales of [Shamwow](https://www.shamwow.com/) in 200 different markets.

{{< youtube 80ne1qRoHyk >}}

Our goal is to improve sales of the Shamwow. Since we cannot directly increase sales of the product (unless we go out and buy it ourselves), our only option is to increase advertising across three potential mediums: internet, newspaper, and TV.

In this example, the advertising budgets are our **input variables**, also called **independent variables**, **features**, or **predictors**. The sales of Shamwows is the **output**, also called the **dependent variable** or **response**.

By plotting the variables against one another using a scatterplot, we can see there is some sort of relationship between each medium's advertising spending and Shamwow sales:



<img src="/notes/statistical-learning_files/figure-html/plot_ad-1.png" width="672" />

But there seems to be a lot of noise in the data. How can we summarize this? We can do so by estimating a mathematical equation following the general form:

$$Y = f(X) + \epsilon$$

where $f$ is some fixed, unknown function of the relationship between the independent variable(s) $X$ and the dependent variable $Y$, with some random error $\epsilon$.

Statistical learning refers to the set of approaches for estimating $f$. There are many potential approaches to defining the functional form of $f$. One approach widely used is called **least squares** - it means that the overall solution minimizes the sum of the squares of the errors made in the results of the equation. The errors are simply the vertical difference between the actual values for $y$ and the predicted values for $y$. Applied here, the results would look like:

<img src="/notes/statistical-learning_files/figure-html/plot_ad_fit-1.png" width="672" />

However statistical learning (and machine learning) allows us to use a wide range of functional forms beyond a simple linear model.

## Why estimate $f$?

There are two major goals of statistical modeling:

## Prediction

Under a system of **prediction**, we use our knowledge of the relationship between $X$ and $Y$ to predict $Y$ for given values of $X$. Often the function $f$ is treated as a **black box** - we don't care what the function is, as long as it makes accurate predictions. If we are trying to boost sales of Shamwow, we may not care why specific factors drive an increase in sales - we just want to know how to adjust our advertising budgets to maximize sales.

## Inference

Under a system of **inference**, we use our knowledge of $X$ and $Y$ to understand the relationship between the variables. Here we are most interested in the explanation, not the prediction. So in the Shamwow example, we may not care about actual sales of the product - instead, we may be economists who wish to understand how advertising spending influences product sales. We don't care about the actual product, we simply want to learn more about the process and **generalize** it to a wider range of settings.

## How do we estimate $f$?

There are two major approaches to estimating $f$: parametric and non-parametric methods.

## Parametric methods

Parametric methods involve a two-stage process:

1. First make an assumption about the functional form of $f$. For instance, OLS assumes that the relationship between $X$ and $Y$ is **linear**. This greatly simplifies the problem of estimating the model because we know a great deal about the properties of linear models.
1. After a model has been selected, we need to **fit** or **train** the model using the actual data. We demonstrated this previously with ordinary least squares. The estimation procedure minimizes the sum of the squares of the differences between the observed responses $Y$ and those predicted by a linear function $\hat{Y}$.

<img src="/notes/statistical-learning_files/figure-html/plot_parametric-1.png" width="672" />

This is only one possible estimation procedure, but is popular because it is relatively intuitive. This model-based approach is referred to as **parametric**, because it simplifies the problem of estimating $f$ to estimating a set of parameters in the function:

$$Y = \beta_0 + \beta_{1}X_1$$

where $Y$ is the sales, $X_1$ is the advertising spending in a given medium (internet, newspaper, or TV), and $\beta_0$ and $\beta_1$ are parameters defining the intercept and slope of the line.

The downside to parametric methods is that they assume a specific functional form of the relationship between the variables. Sometimes relationships really are linear - often however they are not. They could be curvilinear, parbolic, interactive, etc. Unless we know this *a priori* or test for all of these potential functional forms, it is possible our parametric method will not accurately summarize the relationship between $X$ and $Y$.

## Non-parametric methods

Non-parametric methods do not make any assumptions about the functional form of $f$. Instead, they use the data itself to estimate $f$ so that it gets as close as possible to the data points without becoming overly complex. By avoiding any assumptions about the functional form, non-parametric methods avoid the issues caused by parametic models. However, by doing so non-parametric methods require a large set of observations to avoid **overfitting** the data and obtain an accurate estimate of $f$.

One non-parametric method is called **$K$-nearest neighbors regression** (KNN regression). Rather than binning the data into discrete and fixed intervals, KNN regression uses a moving average to generate the regression line. Given a value for $K$ and a prediction point $x_0$, KNN regression identifies the $K$ observations closest to the prediction point $X_0$, and estimates a local regression line that is the average of these observations values for the outcome $Y$.

With $K=1$, the resulting KNN regression line will fit the training observations extraordinarily well.

<img src="/notes/statistical-learning_files/figure-html/knn-1-1.png" width="672" />

Perhaps a bit too well. Compare this to $K=9$:

<img src="/notes/statistical-learning_files/figure-html/knn-9-1.png" width="672" />

This smoothing line averages over the nine nearest observations; while still a step function, it is smoother than $K=1$.

## Classification vs. regression

Variables can be classified as **quantitative** or **qualitative**. Quantitative variables take on numeric values. In contrast, qualitative variables take on different **classes**, or discrete categories. Qualitative variables can have any number of classes, though binary categories are frequent:

* Yes/no
* Male/female

Problems with a quantitative dependent variable are typically called **regression** problems, whereas qualitative dependent variables are called **classification** problems. Part of this distinction is merely semantic, but different methods may be employed depending on the type of response variable. For instance, you would not use linear regression on a qualitative response variable. Conceptually, how would you define a linear function for a response variable that takes on the values "male" or "female"? It doesn't make any conceptual sense. Instead, you can employ classification methods such as **logistic regression** to estimate the probability that based on a set of predictors a specific observation is part of a response class.

That said, whether **predictors** are qualitative or quantitative is not important in determining whether the problem is one of regression or classification. As long as qualitative predictors are properly coded before the analysis is conducted, they can be used for either type of problem.

### Session Info



```
## ─ Session info ───────────────────────────────────────────────────────────────
##  setting  value                       
##  version  R version 3.6.1 (2019-07-05)
##  os       macOS Catalina 10.15.3      
##  system   x86_64, darwin15.6.0        
##  ui       X11                         
##  language (EN)                        
##  collate  en_US.UTF-8                 
##  ctype    en_US.UTF-8                 
##  tz       America/Chicago             
##  date     2020-02-18                  
## 
## ─ Packages ───────────────────────────────────────────────────────────────────
##  package     * version date       lib source        
##  assertthat    0.2.1   2019-03-21 [1] CRAN (R 3.6.0)
##  backports     1.1.5   2019-10-02 [1] CRAN (R 3.6.0)
##  blogdown      0.17.1  2020-02-13 [1] local         
##  bookdown      0.17    2020-01-11 [1] CRAN (R 3.6.0)
##  broom         0.5.4   2020-01-27 [1] CRAN (R 3.6.0)
##  callr         3.4.2   2020-02-12 [1] CRAN (R 3.6.1)
##  cellranger    1.1.0   2016-07-27 [1] CRAN (R 3.6.0)
##  cli           2.0.1   2020-01-08 [1] CRAN (R 3.6.0)
##  codetools     0.2-16  2018-12-24 [1] CRAN (R 3.6.1)
##  colorspace    1.4-1   2019-03-18 [1] CRAN (R 3.6.0)
##  crayon        1.3.4   2017-09-16 [1] CRAN (R 3.6.0)
##  DBI           1.1.0   2019-12-15 [1] CRAN (R 3.6.0)
##  dbplyr        1.4.2   2019-06-17 [1] CRAN (R 3.6.0)
##  desc          1.2.0   2018-05-01 [1] CRAN (R 3.6.0)
##  devtools      2.2.1   2019-09-24 [1] CRAN (R 3.6.0)
##  digest        0.6.23  2019-11-23 [1] CRAN (R 3.6.0)
##  dplyr       * 0.8.4   2020-01-31 [1] CRAN (R 3.6.0)
##  ellipsis      0.3.0   2019-09-20 [1] CRAN (R 3.6.0)
##  evaluate      0.14    2019-05-28 [1] CRAN (R 3.6.0)
##  fansi         0.4.1   2020-01-08 [1] CRAN (R 3.6.0)
##  farver        2.0.3   2020-01-16 [1] CRAN (R 3.6.0)
##  FNN         * 1.1.3   2019-02-15 [1] CRAN (R 3.6.0)
##  forcats     * 0.4.0   2019-02-17 [1] CRAN (R 3.6.0)
##  fs            1.3.1   2019-05-06 [1] CRAN (R 3.6.0)
##  generics      0.0.2   2018-11-29 [1] CRAN (R 3.6.0)
##  ggplot2     * 3.2.1   2019-08-10 [1] CRAN (R 3.6.0)
##  glue          1.3.1   2019-03-12 [1] CRAN (R 3.6.0)
##  gtable        0.3.0   2019-03-25 [1] CRAN (R 3.6.0)
##  haven         2.2.0   2019-11-08 [1] CRAN (R 3.6.0)
##  here        * 0.1     2017-05-28 [1] CRAN (R 3.6.0)
##  hms           0.5.3   2020-01-08 [1] CRAN (R 3.6.0)
##  htmltools     0.4.0   2019-10-04 [1] CRAN (R 3.6.0)
##  httr          1.4.1   2019-08-05 [1] CRAN (R 3.6.0)
##  jsonlite      1.6.1   2020-02-02 [1] CRAN (R 3.6.0)
##  knitr         1.28    2020-02-06 [1] CRAN (R 3.6.0)
##  labeling      0.3     2014-08-23 [1] CRAN (R 3.6.0)
##  lattice       0.20-38 2018-11-04 [1] CRAN (R 3.6.1)
##  lazyeval      0.2.2   2019-03-15 [1] CRAN (R 3.6.0)
##  lifecycle     0.1.0   2019-08-01 [1] CRAN (R 3.6.0)
##  lubridate     1.7.4   2018-04-11 [1] CRAN (R 3.6.0)
##  magrittr      1.5     2014-11-22 [1] CRAN (R 3.6.0)
##  memoise       1.1.0   2017-04-21 [1] CRAN (R 3.6.0)
##  modelr        0.1.5   2019-08-08 [1] CRAN (R 3.6.0)
##  munsell       0.5.0   2018-06-12 [1] CRAN (R 3.6.0)
##  nlme          3.1-144 2020-02-06 [1] CRAN (R 3.6.0)
##  pillar        1.4.3   2019-12-20 [1] CRAN (R 3.6.0)
##  pkgbuild      1.0.6   2019-10-09 [1] CRAN (R 3.6.0)
##  pkgconfig     2.0.3   2019-09-22 [1] CRAN (R 3.6.0)
##  pkgload       1.0.2   2018-10-29 [1] CRAN (R 3.6.0)
##  prettyunits   1.1.1   2020-01-24 [1] CRAN (R 3.6.0)
##  processx      3.4.1   2019-07-18 [1] CRAN (R 3.6.0)
##  ps            1.3.0   2018-12-21 [1] CRAN (R 3.6.0)
##  purrr       * 0.3.3   2019-10-18 [1] CRAN (R 3.6.0)
##  R6            2.4.1   2019-11-12 [1] CRAN (R 3.6.0)
##  Rcpp          1.0.3   2019-11-08 [1] CRAN (R 3.6.0)
##  readr       * 1.3.1   2018-12-21 [1] CRAN (R 3.6.0)
##  readxl        1.3.1   2019-03-13 [1] CRAN (R 3.6.0)
##  remotes       2.1.0   2019-06-24 [1] CRAN (R 3.6.0)
##  reprex        0.3.0   2019-05-16 [1] CRAN (R 3.6.0)
##  rlang         0.4.4   2020-01-28 [1] CRAN (R 3.6.0)
##  rmarkdown     2.1     2020-01-20 [1] CRAN (R 3.6.0)
##  rprojroot     1.3-2   2018-01-03 [1] CRAN (R 3.6.0)
##  rstudioapi    0.11    2020-02-07 [1] CRAN (R 3.6.0)
##  rvest         0.3.5   2019-11-08 [1] CRAN (R 3.6.0)
##  scales        1.1.0   2019-11-18 [1] CRAN (R 3.6.0)
##  sessioninfo   1.1.1   2018-11-05 [1] CRAN (R 3.6.0)
##  stringi       1.4.5   2020-01-11 [1] CRAN (R 3.6.0)
##  stringr     * 1.4.0   2019-02-10 [1] CRAN (R 3.6.0)
##  testthat      2.3.1   2019-12-01 [1] CRAN (R 3.6.0)
##  tibble      * 2.1.3   2019-06-06 [1] CRAN (R 3.6.0)
##  tidyr       * 1.0.2   2020-01-24 [1] CRAN (R 3.6.0)
##  tidyselect    1.0.0   2020-01-27 [1] CRAN (R 3.6.0)
##  tidyverse   * 1.3.0   2019-11-21 [1] CRAN (R 3.6.0)
##  usethis       1.5.1   2019-07-04 [1] CRAN (R 3.6.0)
##  utf8          1.1.4   2018-05-24 [1] CRAN (R 3.6.0)
##  vctrs         0.2.2   2020-01-24 [1] CRAN (R 3.6.0)
##  withr         2.1.2   2018-03-15 [1] CRAN (R 3.6.0)
##  xfun          0.12    2020-01-13 [1] CRAN (R 3.6.0)
##  xml2          1.2.2   2019-08-09 [1] CRAN (R 3.6.0)
##  yaml          2.2.1   2020-02-01 [1] CRAN (R 3.6.0)
## 
## [1] /Library/Frameworks/R.framework/Versions/3.6/Resources/library
```
