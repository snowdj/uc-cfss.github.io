---
title: "HW09: Geospatial visualization"
date: 2019-05-20T13:30:00-06:00  # Schedule page publish date
publishdate: 2019-04-01

draft: true
type: post
aliases: ["/hw07-geospatial.html"]

summary: "Build a map."
url_code: "https://github.com/cfss-win21/hw09"
---



# Overview

Due by class on December 3rd.

# Fork the `hw07` repository

Go [here](https://github.com/cfss-win21/hw09) to fork the repo.

# Generate a geospatial visualization

Your objective: build a map.

## Is that really all the help I get?

Yes.

## Arrrrrrrrgh but that is so vague

I know. But at this point you should be able to rise to the occasion.

## But where do I start?

Think of data you've seen in the past that you think would make for a good geospatial visualization. Which means it needs to include both a geographic component plus some additional data to overlay on top of the geography.

As for drawing the geographic boundaries, that depends on what you want to map. Find a relevant shapefile or GeoJSON which contains the boundaries for the region you wish to visualize. [Google is a great starting point](https://www.google.com/search?q=where+to+get+shapefiles). If you need help finding a relevant shapefile, feel free to post on the issues page to get help from the instructional staff/peers.

{{% callout note %}}

Some suggested sources compiled by Deblina Mukherjee, a previous TA for the course:

* City Open Data Portals - Check [here](http://us-cities.survey.okfn.org/) to see what any city has available.
* [The government more generally.](https://catalog.data.gov/dataset?metadata_type=geospatial) Geospatial data is kind of hard to generate individually. Also consider [the `tidycensus` package](/notes/vector-maps-practice/) for any data related to the United States and demographics.
* The [Array of Things](https://arrayofthings.github.io/) - this is a project led by UChicago faculty. It contains lots of different data from independent street-level sensors, all based in Chicago. 
* [The Center for Spatial Data Science](https://geodacenter.github.io/data-and-lab/). Also very UChicago -- but **fair warning, all of these files are clean**. Since they are relatively easy to implement, I would expect more effort applied on customizing their appearance and telling a story with them.
* [The UChicago Library](https://www.lib.uchicago.edu/e/collections/maps/chigis.html)

{{% /callout %}}

Once you have your geographic boundaries data (either from an R package or imported from an external file), combine this with your substantive data you wish to visualize. Be sure to make the graph presentable - that is, make it look like a nice map. Things to consider include (but are not limited to):

* A map projection system
* Appropriate legends, titles, labels, etc.
* Color palette

**Along with the map itself, write a brief description (250-500 words) of the map.** Summarize the information being depicted and explain any major visual design choices (e.g. why this color palette, why split the continuous variable into XYZ intervals rather than ABC intervals).

{{% callout note %}}

Remember to make your assignment reproducible. If you get a shapefile from the internet, either include it in your repo or make sure your R Markdown document/R script includes a function to download it from the internet.

{{% /callout %}}

# Submit the assignment

Your assignment should be submitted an R Markdown document using the `github_document` format. Follow instructions on [homework workflow](/faq/homework-guidelines/#homework-workflow). As part of the pull request, you're encouraged to reflect on what was hard/easy, problems you solved, helpful tutorials you read, etc.

# Rubric

Needs improvement: Cannot get code to run or is poorly documented. No documentation in the `README` file. Severe misinterpretations of the results. Overall a shoddy or incomplete assignment. Map looks amateurish or hard to interpret.

Satisfactory: Solid effort. Hits all the elements. No clear mistakes. Easy to follow (both the code and the output). Nothing spectacular, either bad or good.

Excellent: Interpretation is clear and in-depth. Accurately interprets the results, with appropriate caveats for what the technique can and cannot do. Code is reproducible. Writes a user-friendly `README` file. Graph looks crisp, easy-to-read, and communicates information honestly and accurately.
