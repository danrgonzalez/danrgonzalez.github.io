---
title: "Group Fitness Benchmark Analytics"
description: "Weekend data project on crowdsourced results from recurring studio benchmark workouts: messy multi-year survey data, feature engineering, and regression."
categories: [Python, pandas, EDA, Regression]
order: 3
---

A weekend data project built on crowdsourced results from recurring group-fitness benchmark workouts: rowing time trials, timed runs, and a row/floor/run triathlon.

- **Messy data, one schema.** Several years of community survey exports, each with different question wording and columns, are harmonized into one schema per benchmark in pandas. Implausible entries are filtered out with fixed plausibility bounds on self-reported height and weight.
- **Feature engineering.** A standard ergometer pace-to-watts conversion turns split pace into watts and watts per kilogram so athletes of different sizes can be compared. Triathlon results are broken into row, floor, and run segment times.
- **Analysis.** Correlation heatmaps, pair plots, and a simple linear regression of 2000 m row time on height, with the coefficient expressed in seconds per inch.

[Python · Jupyter · pandas · NumPy · seaborn · scikit-learn]{.stack}

*Notebooks are private because the underlying data includes other people's self-reported information.*

[← All projects](../projects.qmd){.back-link}
