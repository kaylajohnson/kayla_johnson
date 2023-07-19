---
title: "How to get R help"
subtitle: ""
excerpt: "How to help yourself get help with R"
date: 2022-01-03
author: "Kayla Johnson"
draft: true
images:
series:
tags:
  - R
categories:
  - R
layout: single
---



This post is a reworking of a presentation I gave for R-Ladies East Lansing. I go through my general debugging workflow, then talk about what to do when that fails and you need outside help. 

## Debugging

Eventually you run some code you expect to work and instead get an error. When this happens to me, these are my general steps.

### Reread the code and check for typos

Is there a missing comma? Misspelled argument? As obvious as this should be, I have occasionally Googled an error message only to come back to my code and realize it was a simple typo. Reread first!

### Check that I used the function correctly

If you can pinpoint the function causing the error, it is a good idea to look that function up and check that the expected input, arguments, and output match your expectations. The easiest way to do this is to run `?function_name` in your R console in R Studio. The help page for the function will pop up in the viewing pane. This help page has a lot of important and helpful information, usually including the following sections: *Description* (what the function does), *Usage* (how to use function, with default arguments), *Arguments* (detailed description and type for each argument), *Value* (what the function returns), *References* (relevant material to help understand the function, especially helpful for statistical methods), *See Also* (similar functions, which could be better suited for your issue), and *Examples* (code using demonstrating use of function). 

### Search for the error message

Copy and paste that error message right into the search bar. You will likely see Stack Overflow posts, GitHub issues, or even email chains dedicated to an error message like yours. 

### Google "how to..." and compare similar code to yours

If searching for the error message didn't help, sometimes searching more generally for how to do what you are trying to accomplish will bring you code that does exactly that. Figuring out what exactly to search for is a skill that can be honed, but it is also a bit of an art. Here are a few tips for your Google searches:

- Add "R" to your searches. People do data science in multiple languages, this helps restrict to R.
- Add the package and/or function you are working with (details!)
  “How to remove rows based on multiple columns R” → “How to **filter** data frames across multiple columns at once **dplyr**”
- Try Googling slightly different phrasing (including removing your specific function/package details)
- At least glance at the most relevant Stack Overflow example that pops up and see if the solution can be applied to your problem (it will not always be immediately obvious)

### Ask a question on public forum

Debugging on your own didn't work out, time to ask others for help! The next section covers how and where you should do this.

## Asking for help

The first part in asking for help is figuring out _where_ to ask for help. If you're asking for help fixing code, the second part is making your problem **easily reproducible** so that someone else can reproduce and help fix your problem. 

### Where to ask questions

There are several places you can ask for R help, but here are the most popular:

- **Stack Overflow**  [tour link](https://stackoverflow.com/tour)
Best for programming questions (of any language). Click the tour link to learn about how the site works.

- **Stack Exchange** [all sites in stack exchange network](https://stackexchange.com/sites)
A network of sites (Stack Overflow is one), you find the correct site for your question. All kinds of topics are covered by these sites, but one example of interest to data scientists would be the Cross Validated Stack Exchange, which is “Q&A for people interested in statistics, machine learning, data analysis, data mining, and data visualization.” Click the all sites link above to explore other topic sites.

- **Posit Community (formerly RStudio Community)** [link](https://community.rstudio.com/)
This site is currently in transition due to the RStudio company changing name to Posit, but the site typically has many categories/tags including RStudio, tidyverse, shiny, R markdown, package development, teaching, etc.

- **rOpenSci** [discuss link](https://discuss.ropensci.org/)
For questions relating to open science, reproducibility, documenting data, extracting data, package development, etc. Click the link to check it out!

- **Other Field-specific Forums** 
Forums created for specific fields of study can also be very helpful when you have a niche question. Examples include [R mailing lists](https://www.r-project.org/mail.html) or [Bioconductor support](https://support.bioconductor.org/) for computational biology R packages.

If you have a hard time deciding where your question should be asked, that is ok! Sometimes getting help means posting in the wrong forum and being redirected to the proper place to ask your question. For the most part, the R community is helpful and supportive, so don't take being redirected as bad thing, it is just a way to get you the best chance of help.

### Creating a reproducible example

There are two necessary considerations for creating a reproducible example that will get you help:

(1) Your code must be **easily reproducible** for someone else on their computer. If they cannot reproduce your error, they may not be able to help. If you do not provide an example there is nothing for them to play with to figure out your problem. So, you need to capture everything: include code you tried, any library() calls, and create all necessary objects (such as workable data).

(2) The reproducible example needs to be **complete but minimal**. Strip away everything that is not directly related to your problem. This usually involves creating a much smaller and simpler R object than the one you’re facing in real life.



