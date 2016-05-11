---
layout: post
title: Metis - Project Luther
---

An Application of Linear Regression to Scraped Movie Data

## Low-Budget Films with High Returns: What makes them so special? (and profitable)

### Introduction

Using Linear Regression and variable selection techniques, I will investigate the association between highly successful low-budget films and a set of variables that may explain their huge success. The data for this model is scraped from several movie review websites. 
 
### Motivation

We could look at the financial success of all movies and build a model accordingly. However, due to resource constraints, we will restrict this study to low-budget films. A film's success will be measured by its return on investment or ROI. Given this, we wish to build a model that associates ROI with the characteristics of a movie.
 
### Definitions

In this study, we will focus on movies with budgets under $2M dollars. We define Return on Investment (ROI) as: (Revenue – Budget)/Budget.

### Data

Since we strive to find associations between ROI and movie characteristics. Possible characteristics or features that we may include in our model are:

- Critic Scores
- Runtime
- Widest Release
- Genre
- MPAA Rating
- Distributor

To gather this data, the following sites were scraped using BeautifulSoup in Python:

- Revenue and Budgets: The-Numbers.com
- Critic Scores: RottenTomatoes.com
- Others: BoxOfficeMojo.com

~300 data points were gathered for the model. 

### Analysis

Based on the above features, let’s construct a preliminary linear regression model: 

ROI = Critic Scores + Runtime + Widest Release + Genre + MPAA Rating + Distributor

As we will see in our results, not all of these variables are meaningful and are subsequently dropped from the model by variable selection techniques. For example, there is absolutely no correlation between Runtime and ROI, as expected. 

### Results

Using Stepwise variable selection, the following features remain in the model:

Worldwide_ROI_LOG = Widest_Release_T + Horror + Thriller + Columbia

Horror and Thriller are variables derived from the Genre category and Columbia from the Distributor category.

This model achieved an Adjusted R^2 : 0.31. All variables in the model are statistically significant at a significance level of 0.03.

Visually, here are the associations between ROI and the variables in the model:

![Associations]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Screen Shot 2016-05-10 at 3.36.18 PM.png)

Model Summary:

![Model Statistics]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Screen Shot 2016-05-10 at 3.37.20 PM.png)

### Summary

In summary, should we invest based on this model? No.

But if you really want to, aim at making a Horror film without any Thrill, that is distributed by Columbia with the widest release possible.

In the meantime, let’s dig for more data (features) or we re-evaluate how we approach this particular question to arrive at a better model.

### Additional Thoughts

Stepwise Variable Selection was used to eliminate insignificant variables and optimize our Adjusted R^2

Running a CVLasso (SciKit Learn) on this Model yields a lambda = 0.001, indicating that the model warrants no additional cross validation penalties. 

Here is a link to view the presentation given at Metis:
[PDF of Presentation]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Blog_Presentation_Luther_DRG.pdf)
