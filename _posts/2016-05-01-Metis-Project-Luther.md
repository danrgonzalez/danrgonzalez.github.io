---
layout: post
title: Metis - Project Luther
---

An Application of Linear Regression to Scraped Movie Data

## Low-Budget Movies with High ROIs: What makes them so special? (and profitable)

### Introduction

Using Linear Regression and variable selection techniques, we will investigate the association between high ROI movies that report budgets under $2M and the variables that make them such a huge success. The data for this model is gathered from several movie-related websites. 
 
### Definitions

In this study, we will focus on movie budgets under $2M dollars. We seek to maximize Return on Investment (ROI), with ROI defined as: (Revenue – Budget)/Budget.

### Data

In summary, we strive to find associations between high ROI movies and features that characterize the movie. Possible Features include:

- Critic Scores
- Runtime
- Widest Release
- Genre
- MPAA Rating
- Distributor

To collect the data, the following websites were scraped:

- ROI : The-Numbers.com
- Critic Scores : RottenTomatoes.com
- Others: BoxOfficeMojo.com

### Analysis

Let’s try a linear regression model to associate ROI with these features. A potential model is as follows: 

ROI = Critic Scores + Runtime + Release Size + Genre + MPAA + Distributor

### Results

Using Stepwise variable selection, the following features remained in the model:

Worldwide_ROI_LOG = Widest_Release_T + Horror + Thriller + Columbia

Visually, these are the associations:

![Associations]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Screen Shot 2016-05-10 at 3.36.18 PM.png)

![Model Statistics]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Screen Shot 2016-05-10 at 3.37.20 PM.png)

The model above achieved an Adjusted R2 : 0.31. All variables in the final model are statistically significant (significance level: 0.03).

### Summary

Should you invest based on this model? No.

But if you want to, aim at making a Horror film without any Thrill, that is distributed by Columbia with the widest release possible

In the meantime, let’s dig for more data or we re-evaluate how we approach this particular question.

### Additional Thoughts

Utilized a Stepwise Variable Selection Techniques to eliminate insignificant variables and optimize our Adjusted R2
Running a CVLasso (SciKit Learn) on this Model yields a lambda = 0.001

[PDF of Presentation]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Blog_Presentation_Luther_DRG.pdf)
