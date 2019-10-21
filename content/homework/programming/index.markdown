---
title: "HW04: Programming in R"
date: 2019-04-22T13:30:00-06:00  # Schedule page publish date
publishdate: 2019-03-01

draft: false
type: post
aliases: ["/hw04-programming.html"]

summary: "Implement elemental programming techniques in both contrieved and real-world scenarios."
url_code: "https://github.com/cfss-fa19/hw04"
---



# Overview

Due before class on October 29th.

# Fork the `hw04` repository

Go [here](https://github.com/cfss-fa19/hw04) to fork the repo.

# Part 1: Programming exercises

1. Compute the number of unique values in each column of `iris`.
    1. Write code that uses a `for` loop to do this task.
    1. Write code that uses one of the `map` functions to do this task.
1. Calculate the square of each element in vector `x`:

    
    ```
    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
    ## [24] 24 25 26 27 28 29 30
    ```
    
    1. Write code that uses a `for` loop to do this task.
    1. Write code that uses one of the `map` functions to do this task.
1. [Pythagorean theorem](https://en.wikipedia.org/wiki/Pythagorean_theorem)
    `$$a^2 + b^2 = c^2$$`
    * Write a function that, given the lengths of two sides of the triangle, calculates the length of the third side
    * This function should be flexible - that is, the function works if I give it values for `\(a\)` and `\(b\)`, or `\(b\)` and `\(c\)`, or `\(a\)` and `\(c\)`
    * If the user only provides the length of one side, the function should [throw an error with `stop()`](http://r4ds.had.co.nz/functions.html). Likewise, if the user provides the lengths of all three sides, the function should throw an error.
    * If the user provides any values other than numeric values, the function should throw an error

# Part 2: Using programming in data analysis

The World Bank publishes extensive socioeconomic data on countries/economies around the world. In the `data_world_bank` folder, I have downloaded the World Bank's [complete economic indicators](https://data.worldbank.org/indicator) for each economy.^[See [the documentation](https://datahelpdesk.worldbank.org/knowledgebase/articles/378834-how-does-the-world-bank-classify-countries) for more information on how the World Bank defines a country or an economy.]

In order to analyze the data, you first need to import it. Each economy's data is stored in a separate `.csv` file. You should write a function which uses one argument (the filepath to the data file). Given this path, the function should read and tidy the economy data, and return the cleaned data frame as the output. Remember the rules for a tidy data frame:

1. Each variable must have its own column.
1. Each observation must have its own row.
1. Each value must have its own cell.

Since the World Bank has hundreds of indicators, your function should pare this down to only the handful of variables you intend to analyze.

#### Let's recap the requirements for your function

* Give it a meaningful name
* It should take a single argument - the filepath to the data file. [Chapter 8 of *R for Data Science*](http://r4ds.had.co.nz/workflow-projects.html) explains directory structures and how to access files in a project. **Review this.** You will be marked down if your function requires an absolute file path such as `/Users/soltoffbc/Projects/Computing for Social Sciences/teach/homework/hw04`
* The output of the function should be a **tidy data frame for a single country, pared down to 2-4 substantive variables you will analyze**.

Once you have written this function, demonstrate that it works by importing the data files and combining them into a single data frame using an iterative operation.

Once you have the data imported, write a brief report exploring and analyzing at least [two variables in the data](http://data.worldbank.org/indicator). You should explore both variance and covariance, and present your results and analysis in a coherent and interpretable manner. The main point is that your report should not just be code and output from R - you also need to include your own written analysis. Submitting the report as an [R Markdown document](http://rmarkdown.rstudio.com/) will make this much easier (and is in fact mandatory).

# Submit the assignment

Your assignment should be submitted as two RMarkdown documents. Follow instructions on [homework workflow](/faq/homework-guidelines/#homework-workflow). As part of the pull request, you're encouraged to reflect on what was hard/easy, problems you solved, helpful tutorials you read, etc.

# Rubric

Check minus: Displays minimal effort. Doesn't complete all components. Code is poorly written and not documented. World Bank analysis is slapped together, or just contains code and output. No history of commits to document work.

Check: Solid effort. Hits all the elements. No clear mistakes. Easy to follow (both the code and the output). Nothing spectacular, either bad or good.

Check plus: Functions are written succinctly and comprehensibly. Error checks are incorporated into functions as appropriate. Code is written parsimoniously and properly formatted. Frequent use of commits to track changes. Exploratory analysis demonstrates thought and consideration of the relationships. Graphs and tables are properly labeled. Descriptive text is incorporated into the World Bank analysis that explains what you are examining and how to interpret the results.
