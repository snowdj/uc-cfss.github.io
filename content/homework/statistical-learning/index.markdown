---
title: "HW07: Statistical learning"
date: 2019-05-06T13:30:00-06:00  # Schedule page publish date
publishdate: 2019-04-01

draft: false
type: post
aliases: ["/hw06-stat-learn.html"]

summary: "Implement statistical learning methods for regression and classification."
url_code: "https://github.com/cfss-fa19/hw05"
---



# Overview

Due before class on November 19th.

# Fork the `hw05` repository

Go [here](https://github.com/cfss-fa19/hw05) to fork the repo.

# Part 1: President Donald Trump

![President Donald Trump's official portrait](https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Donald_Trump_official_portrait.jpg/606px-Donald_Trump_official_portrait.jpg)

<iframe width='480' height='290' scrolling='no' src='https://www.washingtonpost.com/video/c/embed/3bf16d1e-8caf-11e6-8cdc-4fbb1973b506' frameborder='0' webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

Donald Trump was certainly an unconventional candidate for the U.S. presidency. His victory was even more shocking in the context of [a taped recording from 2005 released just four weeks before the election](https://www.washingtonpost.com/politics/trump-recorded-having-extremely-lewd-conversation-about-women-in-2005/2016/10/07/3b9ce776-8cb4-11e6-bf8a-3d26847eeed4_story.html?utm_term=.cb543aad44f9). In the video, Trump brags in vulgar terms about kissing, groping, and trying to have sex with women. Many politicians and pundits identified this incident as the end of the Trump campaign. Yet surprisingly, it seemed to have little impact on the election.

Using statistical learning and data from the [2012 American National Election Studies survey](http://www.electionstudies.org/), evaluate to what extent this video influenced individual attitudes towards then-candidate Trump. The outcome of interest is a **feeling thermometer rating** of Trump. Feeling thermometers are a common metric in survey research used to gauge attitudes or feelings of warmth towards individuals and institutions. They range from 0-100, with 0 indicating extreme coldness and 100 indicating extreme warmth.

Specifically, how do individual perceptions of the importance of this video effect feelings of warmth towards the candidate? `trump.csv` contains a selection of variables from the larger survey that also allow you to test competing factors that may influence attitudes towards Donald Trump.

* `trump` - ranges from 0-100
* `video` - response to the question

    > In deciding how to vote, how much do you think the information from the video should have mattered to people?
    
    * `0` - not at all
    * `1` - a little
    * `2` - a moderate amount
    * `3` - a lot
    * `4` - a great deal
    
* `female` - 1 if individual is female, 0 if individual is male
* `pid` - party identification
    * `0` - Strong Democrat
    * `1` - Not very strong Democrat
    * `2` - Independent-Democrat
    * `3` - Independent
    * `4` - Independent-Republican
    * `5` - Not very strong Republican
    * `6` - Strong Republican
* `age` - age of respondent in years
* `educ` - measure of educational attainment. Some representative values include:
    * `0` - less than 1st grade
    * `8` - high school graduate (or GED)
    * `12` - bachelor's degree
    * `15` - doctoral degree

1. Estimate a basic (single variable) linear regression model of the relationship between the importance of the video and feelings towards Donald Trump. Calculate predicted values, graph the relationship between the two variables using the predicted values, and determine whether there appears to be a significant relationship.
1. Build the best predictive linear regression model of attitudes towards Donald Trump given the variables you have available. In this context, "best" is defined as the model with the lowest MSE. Compare at least three different model formulations (aka different combinations of variables). Use 10-fold cross-validation to avoid a biased estimate of MSE.

# Part 2: Predicting attitudes towards racist college professors

The [General Social Survey](http://gss.norc.org/) is a biannual survey of the American public.^[Conducted by NORC at the University of Chicago.]

> [The GSS gathers data on contemporary American society in order to monitor and explain trends and constants in attitudes, behaviors, and attributes. Hundreds of trends have been tracked since 1972. In addition, since the GSS adopted questions from earlier surveys, trends can be followed for up to 70 years. The GSS contains a standard core of demographic, behavioral, and attitudinal questions, plus topics of special interest. Among the topics covered are civil liberties, crime and violence, intergroup tolerance, morality, national spending priorities, psychological well-being, social mobility, and stress and traumatic events.](http://gss.norc.org/About-The-GSS)

You are going to predict attitudes towards racist college professors. Specifically, each respondent was asked "Should a person who believes that Blacks are genetically inferior be allowed to teach in a college or university?" Given the kerfuffle over Richard J. Herrnstein and Charles Murray's [*The Bell Curve*](https://en.wikipedia.org/wiki/The_Bell_Curve) and the ostracization of Nobel Prize laureate [James Watson](https://en.wikipedia.org/wiki/James_Watson) over his controversial views on race and intelligence, this analysis will provide further insight into the public debate over this issue.

`rcfss::gss_colrac` contain a selection of variables from the 2012 GSS. The outcome of interest `colrac` is a binary variable coded as either `TRUE` (respondent believes the person should be allowed to teach) or `FALSE` (respondent believes the person should not allowed to teach). Documentation for the other predictors (if the variable is not clearly coded) can be viewed [here](https://gssdataexplorer.norc.org/variables/vfilter). You can also run `?gss_colrac` to open a documentation file in R.

{{% alert note %}}

`gss_colrac` is found in version `0.1.7` of `rcfss`. You will need to update your package prior to attempting the exercise. Run `devtools::install_github("uc-cfss/rcfss")` from your console to update to the most recent version.

{{% /alert %}}

1. Load the `gss_colrac` data frame from `library(rcfss)`.
1. Estimate three different logistic regression models with `colrac` as the response variable. You may use any combination of the predictors to estimate these models.
    1. Calculate the 10-fold cross-validation error rate for each model. Which model performs the best?
1. Estimate a decision tree model using all the variables and the default settings for `ctree()`. Interpret the resulting decision tree.
1. Now estimate a random forest model with all available variables. Generate a random forest with 500 trees.
    1. Generate a variable importance plot. Which variables seem the most important?
    1. Calculate the out-of-bag error rate. How does this compare to the 10-fold CV error rate from the logistic regression models?

# Submit the assignment

Your assignment should be submitted as a set of R scripts, R Markdown documents, data files, figures, etc. Follow instructions on [homework workflow](/faq/homework-guidelines/#homework-workflow). As part of the pull request, you're encouraged to reflect on what was hard/easy, problems you solved, helpful tutorials you read, etc.

# Rubric

Check minus: Cannot get code to run or is poorly documented. No documentation in the `README` file. Severe misinterpretations of the results. Overall a shoddy or incomplete assignment.

Check: Solid effort. Hits all the elements. No clear mistakes. Easy to follow (both the code and the output). Nothing spectacular, either bad or good.

Check plus: Interpretation is clear and in-depth. Accurately interprets the results, with appropriate caveats for what the technique can and cannot do. Code is reproducible. Writes a user-friendly `README` file. Discusses the benefits and drawbacks of a specific method. Compares multiple models fitted to the same underlying dataset.
