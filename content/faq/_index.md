---
date: "2018-09-09T00:00:00-05:00"
draft: false
menu:
  faq:
    name: Overview
    weight: 1
title: "FAQ"
toc: true
type: docs
---

## Should I take this course?

Meet some of the types of students you will find in this class.

### Jeri

![Stock photo of Jeri](/img/stock-photos/000022.jpg)

* General background
    * PhD student in sociology
    * Studies the science of science
    * Uses [advanced metrics](http://www.borischen.co/) to set her fantasy football lineup. Seems to be effective as she has won two years in a row.
* Starting points
    * Has been analyzing data in Stata for the past three years
    * Feels comfortable with regression and econometric methods
    * Tried to learn Git on her own once, and quickly became frustrated and gave up
* Needs
    * Will be analyzing a large-scale dataset for her dissertation
    * Wants to produce high-quality visualizations
    * Seeks a reproducible workflow to manage all her exploratory and confirmatory analysis

### Ryan

![Stock photo of Ryan](/img/stock-photos/000284.jpg)

* General background
    * Entering the [MAPSS program](https://mapss.uchicago.edu)
    * Undergraduate degree in journalism
    * Enjoys attending [Weird Al Yankovic concerts](https://www.youtube.com/watch?v=qpMvS1Q1sos)
* Starting points
    * Hasn't taken a statistics class in five years
    * Isn't sure whether to pursue a PhD or go back into the private sector after graduating
    * Took an online course of introduction to R, but hasn't used it in his day-to-day work
* Needs
    * Writing a master's thesis in a single year
    * Expects to analyze a collection of published news articles 
    * Wants to understand code samples he finds online so he can repurpose them for his own work

### Fernando

![Stock photo of Fernando](/img/stock-photos/000232.jpg)

* General background
    * Third-year student in the College
    * Majoring in political science
    * Makes an annual pilgrimage to Comic-Con where he traditionally cosplays as [Spock](https://memory-alpha.fandom.com/wiki/Spock)
* Starting points
    * Has taken MATH 15100 and 15200, plus the departmental methods course
    * Isn't afraid to tackle a new challenge
    * Possesses some experience writing scripts in Stata to automate statistical analysis
* Needs
    * Wants to work as a research assistant on a project exploring the onset of civil conflict
    * Faculty advisor's lab works exclusively in R
    * Needs to start contributing to a new research paper next quarter

### Fang

![Stock photo of Fang](/img/stock-photos/000251.jpg)

* General background
    * Born and raised in Shenzhen, China
    * First time living in the United States
    * Improves her English skills by watching the Great British Bake-Off (but was heartbroken when Mary Berry, Mel, and Sue left)
* Starting points
    * Background in psychology, plans to apply for doctoral programs in marketing
    * Uses a mix of Excel, SPSS, and Matlab
    * Is pursuing the [graduate certificate in computational social science](https://macss.uchicago.edu/content/certificate-current-students)
* Needs
    * Is going to run 300 experiments on Amazon MTurk in the next six months
    * Wants to easily share her analysis notebooks with peers in her research lab
    * Expects to take courses in machine learning and Bayesian statistics which require a background in R

## General description

This course is open to any graduate (or advanced undergraduate) at UChicago. I anticipate drawing students from a wide range of departments such as Sociology, Psychology, Political Science, Comparative Human Development, and MAPSS. Typically these students are looking to learn basic computational and analytical skills they can apply to master's theses or dissertation research.

If you have never programmed before or don't even know what the [shell](/setup/shell/) is, **prepare for a shock**. This class will prove to be immensely beneficial if you stick with it, but that will require you to commit for the full 10 weeks. I do not presume any prior programming experience, so everyone starts from the same knowledge level. I guarantee that the first few weeks and assignments will be rough - but the good news is that they will be rough for everyone! Your classmates are struggling with you and you can lean on one another to get through the worst part of the learning curve.

A highly selective sampling of feedback from past course evaluations:

> I think this class is really well-organized. The homework is craftily designed as a way to solidify the materials learned in class. Dr. Soltoff is wonderful and helpful! He came to class fully prepared and made the lectures enjoyable and productive. I suggest that all Ph.D. students in Political Science (at least in my field), who likes to conduct quantitative research, should choose this class in the first year, because this class can well set students up with a good understanding of programming techniques.

> It's a steep learning curve, but very rewarding.

> This class can set you up really nicely with conversant knowledge in R programming and a large amount of coding materials that are helpful for future research. And it also provides students with a first-hand experience of using some of the cutting edge methods and makes students have a taste of them.

> I'm so so so glad I ended up taking this course. I had a lot of doubts about my own (lack of) skills and my inability to to handle so many things in one quarter. I had a lot of apprehensions about this course but they all quickly disappeared. It's quite honestly been one of the most valuable courses I've taken at this University and I attribute all of that to your excellence as a lecturer. You and the TAs have always been extremely accessible to everyone and I can't appreciate that enough. In other classes, I would've been more hesitant to ask "dumb questions" but you all have made me comfortable doing so, and I have benefited immensely from it.

> It's really damn helpful if you want to do any sort of social science research. It's helpful to know how to do any sort of coding just going into the job market with any degree.

## What do I need for this course?

**You will need to bring a computer to class each day.** Class sessions are a mix of lecture, demonstration, and live coding. It is essential to have a computer so you can follow along and complete the exercises.

## Textbooks/Readings

* [R for Data Science](http://r4ds.had.co.nz/) -- Garrett Grolemund and Hadley Wickham
    * [Hardcover available for purchase online](https://www.amazon.com/R-Data-Science-Hadley-Wickham/dp/1491910399/ref=as_li_ss_tl?ie=UTF8&qid=1469550189&sr=8-1&keywords=R+for+data+science&linkCode=sl1&tag=devtools-20&linkId=6fe0069f9605cf847ed96c191f4e84dd)
    * Open-source online version is available for free

    > Completing the exercises in the book? No official solution manual exists, but several can be found online. I recommend [this version by Jeffrey B. Arnold](https://jrnold.github.io/r4ds-exercise-solutions/). Your exact solutions may vary, but these can be a good starting point.

### Additional resources

* [ggplot2: Elegant Graphics for Data Analysis, 2nd Edition](http://link.springer.com.proxy.uchicago.edu/book/10.1007/978-3-319-24277-4) -- Hadley Wickham
    * Excellent resource for the [`ggplot2`](https://cran.r-project.org/web/packages/ggplot2/index.html) graphics library.
* [Advanced R](http://adv-r.had.co.nz/) -- Hadley Wickham
    * Hardcover available online for around $55, but the online version is free
    * A deep dive into R as a programming language, not just a tool for data science. We will use some chapters in class, but most of this material is best covered on your own after completion of this course
* [An Introduction to Statistical Learning: with Applications in R](http://www-bcf.usc.edu/~gareth/ISL/) -- Gareth James, Daniela Witten, Trevor Hastie, and Robert Tibshirani
    * [Hardcover is available online for around $70](https://www.amazon.com/Introduction-Statistical-Learning-Applications-Statistics/dp/1461471370)
    * Authenticated UChicago students can purchase a [softcover black-and-white edition for $25](http://link.springer.com.proxy.uchicago.edu/book/10.1007%2F978-1-4614-7138-7)^[I don't recommend this since many of the figures rely on the use of color.] or download a PDF copy of the entire book for free
    * Non-UChicago students can find a free PDF of the entire book [from the authors' site](http://www-bcf.usc.edu/~gareth/ISL/ISLR%20Sixth%20Printing.pdf)
* [RStudio Cheatsheets](https://www.rstudio.com/resources/cheatsheets/)
    * Printable cheat sheets for common R tasks and features
    * [Data import](https://github.com/rstudio/cheatsheets/raw/master/source/pdfs/data-import-cheatsheet.pdf)
    * [Data transformation](https://github.com/rstudio/cheatsheets/raw/master/source/pdfs/data-transformation-cheatsheet.pdf)
    * [Data visualization](https://github.com/rstudio/cheatsheets/raw/master/source/pdfs/ggplot2-cheatsheet-2.1.pdf)
    * [RStudio IDE](https://github.com/rstudio/cheatsheets/raw/master/source/pdfs/rstudio-IDE-cheatsheet.pdf)
    * [And more!](https://www.rstudio.com/resources/cheatsheets/)
    
### Resources for under-represented groups in programming

{{% callout note %}}

Thanks to [Angela Li](https://angela-li.github.io/) for compiling these recommendations.

{{% /callout %}}

* [R LGBTQ Twitter](https://twitter.com/r_lgbtq): Affinity group for queer people in the R community -- Twitter often promotes events, panels and talks by and for queer R users. 
* [Gayta Science Twitter](https://twitter.com/gaytascience): Alliance that uses data science techniques to give LGBTQ+ experiences a voice -- Twitter will often share data-driven work concerning the LGBTQ+ community. 
* [RLadies Community Slack](http://bit.ly/rladies-slack): A global programming meetup for non-binary, trans, and female R users. 
* [R Ladies Chicago Slack](https://rladiesinvite.herokuapp.com/): Local Chicago chapter of R Ladies -- hosts events, talks, and projects in the Chicago-land area. 
* [RLadies Remote Twitter](https://twitter.com/RLadiesRemote): Remote chapter of R Ladies -- has Slack coffee chats to discuss programming topics in a supportive environment. 
* [People of Color Code Meetup](https://www.meetup.com/People-of-Color-Code/): A meetup for POC software developers -- has events where POC developers can work on personal projects, collaborate, and learn. 
* [R Forwards](https://forwards.github.io/): A task force set up by the R Foundation to address the under-representation of under-represented groups in the R community -- collects representation data in the R community, produces workshops and teaching materials 
* [R Community Diversity and Inclusion Working Group](https://github.com/RConsortium/RCDI-WG): Working group set up by the R Consortium to encourage and support diversity and inclusion across a variety of events and platforms in the R community 

## Software

By the end of the first week (or even better, before the course starts), you should install the following software on your computer:

* [R](https://www.r-project.org/) - easiest approach is to select [a pre-compiled binary appropriate for your operating system](https://cran.rstudio.com/).
* [RStudio's IDE](https://www.rstudio.com/products/RStudio/) - this is a powerful user interface for programming in R. You could use base R, but you would regret it.
* [Git](https://git-scm.com/) - Git is a [version control system](https://en.wikipedia.org/wiki/Version_control) which is used to manage projects and track changes in computer files. Once installed, it can be integrated into RStudio to manage your course assignments and other projects.

Comprehensive instructions for downloading and setting up this software can be found [here](/setup/).

## How will I be evaluated?

Each week students will complete a series of programming assignments linked to lecture materials. These assignments will generally be due the following week prior to Monday's class. Weekly lab sessions will be held to assist students in completing these assignments. While students are encouraged to assist one another in debugging programs and solving problems in these assignments, it is imperative students also learn how to do this for themselves. That is, **students need to understand, write, and submit their own work.**

Each homework will be evaluated by either myself or the TA, as well as by **two peer reviewers**. Each of you is required to provide two peer reviews for each assignment; failure to complete these reviews will result in a deduction of your final grade.

* [General guidelines for submitting homework](/faq/homework-guidelines/)
* [Evaluation criteria for homework](/faq/homework-evaluations/)
* [How to perform peer review](/faq/peer-evaluations/)
* [How to properly ask for help](/faq/asking-questions/)

## COVID-19-related policies

### UChicago Health Pact

All students on campus are required to adhere to the guidelines in the UChicago Health Pact in order to promote a safe environment in the classroom.

* Secure face coverings must be worn appropriately at all times at all times while in University buildings
* Maintain a distance of 6 feet from others
* Do not attend and in-person class if you feel unwell or are experiencing COVID-19 related symptoms

The complete text of the UChicago Health Pact along with additional information about COVID-19 protocols can be found [here](https://goforward.uchicago.edu/health-requirements/#healthpact).

### Reporting COVID-19 related concerns

Any concerns over inappropriate PPE usage, physical distancing, cleaning/disinfection, or other COVID-19 related public health concerns should be directed to [UCAIR](https://ehs-prd-01.uchicago.edu/ehsa-ucair/InjuryIllnessIndexUOFC-IT.html).

If there is an emergency, call 773-702-8181 or dial 123 on any campus phone.

### Reporting COVID-19 exposure or a confirmed case

If you were potentially exposed to COVID-19 or your COVID-19 test results come back positive, reach out immediately to [C19HealthReport@uchicago.edu](mailto:C19HealthReport@uchicago.edu).

### Recording and deletion policies for academic year 2020-21

The Recording and Deletion Policies for the current academic year can be found in the Student Manual under [Petitions, Audio & Video Recording on Campus](https://studentmanual.uchicago.edu/administrative-policies/additional-administrative-regulations/petitions-audio-video-recording-on-campus/).

* Do not record, share, or disseminate any course sessions, videos, transcripts, audio, or chats.
* Do not share links for the course to those not currently enrolled.
* Any Zoom cloud recordings will be automatically deleted 90 days after the completion of the recording.

### Participation if you get COVID-19

Students who have been exposed to or who are experiencing symptoms of COVID-19 should contact [UChicago Student Wellness](https://wellness.uchicago.edu/) immediately to be tested, and reach out to their area Dean of Students to request accommodations for classes until:

* At least 10 days have passed since symptoms first appeared and;
* At least 3 days (72 hours) have passed since recovery - defined as resolution of fever without the use of fever-reducing medications and improvement in respiratory symptoms (e.g., cough, shortness of breath).

## Statement on diversity, inclusion, and disability

The University of Chicago is committed to diversity and rigorous inquiry from multiple perspectives. The MAPSS, CIR, and Computation programs share this commitment and seek to foster productive learning environments based upon inclusion, open communication, and mutual respect for a diverse range of identities, experiences, and positions.

The University of Chicago is committed to ensuring equitable access to our academic programs and services. Students with disabilities who have been approved for the use of academic accommodations by Student Disability Services (SDS) and need a reasonable accommodation(s) to participate fully in this course should follow the procedures established by SDS for using accommodations. Timely notifications are required in order to ensure that your accommodations can be implemented. Please meet with me to discuss your access needs in this class after you have completed the SDS procedures for requesting accommodations.

* Email: [disabilities@uchicago.edu](mailto:disabilities@uchicago.edu)
* Phone: 773-702-6000

## Acknowledgments

* Stock photos of student learners are synthetically generated by [Generated Photos](https://generated.photos/)
