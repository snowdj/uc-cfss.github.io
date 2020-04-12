---
title: "Statistical learning"
date: 2020-05-18T13:30:00
publishDate: 2019-05-06T13:30:00
draft: false
type: "talk"

aliases: ["/cm011.html", "/syllabus/statistical-learning-regression"]

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
time_end: 2020-05-18T14:50:00
all_day: false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors: []

# Abstract and optional shortened version.
abstract: ""
summary: "Review the goals of statistical learning, introduce methods for linear/logistic regression, and practice working with model objects in R."

# Location of event.
location: "Online"

# Is this a selected talk? (true/false)
selected: false

# Tags (optional).
#   Set `tags: []` for no tags, or use the form `tags: ["A Tag", "Another Tag"]` for one or more tags.
tags: []

# Links (optional).
url_pdf: ""
url_slides: "/slides/statistical-learning/"
url_video: ""
url_code: ""

# Does the content use math formatting?
math: false
---



## Overview

* Review the major goals of statistical learning
* Explain the difference between parametric and non-parametric methods
* Introduce linear models and ordinary least squares regression
* Demonstrate how to estimate a linear model in R using `lm()`
* Demonstrate how to extract model statistics using [`broom`](https://cran.r-project.org/web/packages/broom/index.html) and [`modelr`](https://github.com/hadley/modelr)
* Practice estimating and interpreting linear models
* Demonstrate the use of logistic regression for classification
* Identify methods for assessing classification model accuracy

## Before class

* Read chapters 22-25 in [R for Data Science](http://r4ds.had.co.nz/)
* This is not a math/stats class. In class we will **briefly** summarize how these methods work and spend the bulk of our time on estimating and interpreting these models. That said, you should have some understanding of the mathematical underpinnings of statistical learning methods prior to implementing them yourselves. See below for some recommended readings:

##### For those with little/no statistics training

* Chapters 7-8 of [*OpenIntro Statistics*](https://www.openintro.org/stat/textbook.php?stat_book=os) - an open-source statistics textbook written at the level of an introductory undergraduate course on statistics

##### For those with prior statistics training

* Chapters 2-3, 4.1-3 in [*An Introduction to Statistical Learning*](http://link.springer.com.proxy.uchicago.edu/book/10.1007%2F978-1-4614-7138-7) - a book on statistical learning written at the level of an advanced undergraduate/master's level course

## Class materials

* [Statistical learning: the basics](/notes/statistical-learning/)
* [Linear regression](/notes/linear-models/)
* [Logistic regression](/notes/logistic-regression/)

* [Vignette on `broom`](https://cran.r-project.org/web/packages/broom/vignettes/broom.html)
* [Examples of estimating common statistical models in R](http://www.ats.ucla.edu/stat/dae/)

## What you need to do
