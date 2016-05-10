---
layout: post
title: Project Luther
---

An Application of Linear Regression to Scraped Movie Data

# Metis - Project Luther

## Low-Budget Movies & High ROIs

### Introduction

Using Linear Regression and variable selection techniques, we will investigate the association between high ROI movies that report budgets under $2M and variables that make them such a huge success. Data for this model will be gathered from several websites. 
 
### Definitions

In this study, we will focus on movie budgets under $2M dollars. We seek to maximize ROI, with ROI defined as: (Revenue – Budget)/Budget.

### Data

In summary, we strive to find associations between high ROI movies and features that characterize the movie. Possible Features include:
- Critic Scores
- Runtime
- Widest Release
- Genre
- MPAA Rating
- Distributor

### Analysis

Let’s try a linear regression model to associate ROI with these features. A potential model is as follows. 

ROI ~ Critic Scores + Runtime + Release Size + Genre + MPAA + Distributor

The following websites were scraped for the following features:
- ROI : The-Numbers.com
- Critic Scores : RottenTomatoes.com
- Others: BoxOfficeMojo.com

### Results

Most viable model:

Worldwide_ROI_LOG ~ Widest_Release_T + Horror + Thriller + Columbia

![Associations]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Screen Shot 2016-05-10 at 3.36.18 PM.png)

![Model Statistics]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Screen Shot 2016-05-10 at 3.37.20 PM.png)

The model above achieved an Adjusted R2 : 0.31. 
All variables in the final model are statistically significant (significance level: 0.03).

### Interpretation

### Summary

Should you invest based on this model?
No
But if you want to, aim at making a Horror film without any Thrill, that is distributed by Columbia with the widest release possible
In the meantime, let’s dig for more data
Re-evaluate how we approach our question

### Additional Thoughts
Utilized a Stepwise Variable Selection Techniques to eliminate insignificant variables and optimize our Adjusted R2
Running a CVLasso (SciKit Learn) on this Model
Yields a lambda = 0.001
Our variables do not warrant a penalty


[PDF of Presentation]({{https://github.com/danrgonzalez/danrgonzalez.github.io}}/images/Blog_Presentation_Luther_DRG.pdf)
