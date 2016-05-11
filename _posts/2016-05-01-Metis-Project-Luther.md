---
layout: post
title: Metis - Project Luther
---

An Application of Linear Regression to Scraped Movie Data

## Low-Budget Movies with High Returns on Investment: What makes them so special? (and profitable)

### Introduction

Using Linear Regression and variable selection techniques, I will investigate the association between highly successful low-budget films and a set of variables that may explain their huge success. The data for this model is scraped from several movie review websites. 
 
### Motivation

We could look at the financial success of all movies and build a model accordingly. However, due to resource constraints, we will restrict this study to low-budget films. A film's success will be measured by its return on investment or ROI. Given this, we wish to build a model that associates ROI with the characteristics of a movie.
 
### Definitions

In this study, we will focus on movies with budgets under $2M dollars. We define Return on Investment (ROI) as: (Revenue – Budget)/Budget.

### Data

In summary, we strive to find associations between high ROI movies and features that characterize the movie. Possible Features include:

- Critic Scores
- Runtime
- Widest Release
- Genre
- MPAA Rating
- Distributor

To gather this data, the following sites were scraped using BeautfilSoup in Python:

- Revenue and Budgets: The-Numbers.com
- Critic Scores: RottenTomatoes.com
- Others: BoxOfficeMojo.com

### Analysis

Based on the above features, let’s try a linear regression model to associate ROI with these features: 

ROI = Critic Scores + Runtime + Release Size + Genre + MPAA + Distributor

### Results

Using Stepwise variable selection, the following features remained in the model:

Worldwide_ROI_LOG = Widest_Release_T + Horror + Thriller + Columbia

The model above achieved an Adjusted R2 : 0.31. All variables in the final model are statistically significant (significance level: 0.03).

Visually, here are the associations:

![Associations]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Screen Shot 2016-05-10 at 3.36.18 PM.png)

![Model Statistics]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Screen Shot 2016-05-10 at 3.37.20 PM.png)

### Summary

Should you invest based on this model? No.

But if you want to, aim at making a Horror film without any Thrill, that is distributed by Columbia with the widest release possible

In the meantime, let’s dig for more data or we re-evaluate how we approach this particular question.

### Additional Thoughts

Utilized a Stepwise Variable Selection Techniques to eliminate insignificant variables and optimize our Adjusted R2
Running a CVLasso (SciKit Learn) on this Model yields a lambda = 0.001

[PDF of Presentation]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Blog_Presentation_Luther_DRG.pdf)
