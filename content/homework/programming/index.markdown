---
title: "HW04: Programming in R"
date: 2019-04-22T13:30:00-06:00  # Schedule page publish date
publishdate: 2019-03-01

draft: false
type: post
aliases: ["/hw04-programming.html"]

summary: "Implement elemental programming techniques in both contrieved and real-world scenarios."
url_code: "https://github.com/cfss-sp20/hw04"
---



# Overview

Due by 11:59pm CT (Chicago) on May 4th.

# Fork the `hw04` repository

Go [here](https://github.com/cfss-sp20/hw04) to fork the repo.

# Part 1: Programming exercises

1. Compute the number of unique values in each column of `iris`.
    1. Write code that uses a `for` loop to do this task.
    1. Write code that uses one of the `map` functions to do this task.
1. Calculate the square of each element in vector `x`:

    
    ```
    ##  [1] 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6
    ## [26]  5  4  3  2  1
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

The World Bank publishes extensive socioeconomic data on countries/economies around the world. In the `data_world_bank` folder, I have downloaded the World Bank's [complete economic indicators](https://data.worldbank.org/indicator) for each country in their database.

In order to analyze the data, you first need to import it. Each economy's data is stored in a separate `.csv` file.

You should write a function which imports **a single data file**. The function should use one argument (the filepath to the data file). Given this path, the function should read and tidy the economy data, and return the cleaned data frame as the output. Remember the rules for a tidy data frame:

1. Each variable must have its own column.
1. Each observation must have its own row.
1. Each value must have its own cell.

Since the World Bank has hundreds of indicators, your function should pare this down to only the handful of variables you intend to analyze.

Once you have written this function, demonstrate that it works by importing the data files and combining them into a single data frame using an iterative operation.

#### Let's recap the requirements for your function

* Give it a meaningful name
* It should take a single argument - the filepath to the data file. [Chapter 8 of *R for Data Science*](http://r4ds.had.co.nz/workflow-projects.html) explains directory structures and how to access files in a project. **Review this.** You will be marked down if your function requires an absolute file path such as `/Users/soltoffbc/Projects/Computing for Social Sciences/teach/homework/hw04`
* Do not try and run the iterative operation inside of the function. Technically this can work, but it is far harder to fix errors and write the body of the function if you are performing both tasks simultaneously.
* The output of the function should be a **tidy data frame for a single country, pared down to 2-4 substantive variables you will analyze**.

Once you have the data imported, write a brief report exploring and analyzing at least [two variables in the data](http://data.worldbank.org/indicator). You should explore both variance and covariance, and present your results and analysis in a coherent and interpretable manner. The main point is that your report should not just be code and output from R - you also need to include your own written analysis. Submitting the report as an [R Markdown document](http://rmarkdown.rstudio.com/) will make this much easier (and is in fact mandatory).

# Submit the assignment

Your assignment should be submitted as two R Markdown documents using the `github_document` format. Follow instructions on [homework workflow](/faq/homework-guidelines/#homework-workflow). As part of the pull request, you're encouraged to reflect on what was hard/easy, problems you solved, helpful tutorials you read, etc.

# Rubric

Check minus: Displays minimal effort. Doesn't complete all components. Code is poorly written and not documented. World Bank analysis is slapped together, or just contains code and output. No history of commits to document work.

Check: Solid effort. Hits all the elements. No clear mistakes. Easy to follow (both the code and the output). Nothing spectacular, either bad or good.

Check plus: Functions are written succinctly and comprehensibly. Error checks are incorporated into functions as appropriate. Code is written parsimoniously and properly formatted. Frequent use of commits to track changes. Exploratory analysis demonstrates thought and consideration of the relationships. Graphs and tables are properly labeled. Descriptive text is incorporated into the World Bank analysis that explains what you are examining and how to interpret the results.
