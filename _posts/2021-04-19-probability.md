---
layout: post
title:  "Probability Theory Summary"
date:   2021-04-19 00:51:00 +0200
categories: math
tags: CSE1210
---
{% include math.html %}
<!--more-->

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Terminology](#terminology)
  - [Probability vs Statistics](#probability-vs-statistics)
  - [Numerical summaries](#numerical-summaries)
  - [Five-number summary](#five-number-summary)
  - [Graphical summaries](#graphical-summaries)

## Terminology
* stochastic: having a random probability distribution or pattern that may be analysed statistically but may not be predicted precisely.

## Probability vs Statistics
* **Probability**: Given a explicitly defined model, predict the outcome (accurate).
  * Define the probability of events and their distribution, frequency, relation.
* **Statistics**: Given the observed data, fit a model (estimation).
  * Describe the *observed* data (descriptive statistics)
  * Estimate parameters
  * Test hypotheses

## Numerical summaries
* Average (also known as mean)
* Median (middle number/interval of the distinct set, not necessarily the same as the average)
* Minimum and maximum value (range)
* Lower and upper quartile

## Five-number summary
* Minimum
* Lower quartile (median of the set from the minimum to the median)
* Median
* Upper quartile (median of the set from the median to the maximum)
* Maximum

## Graphical summaries
* Histogram
  * Displays data in intervals, which are determined by a "bin" width
  * Bin width is mostly arbitrary, but there are "sweet spots" that show more useful information than others

![histogram]({{ site.url }}/images/histogram_width.PNG)

* Two peaks indicate that there are likely two different types of eruption
* Scatter plots show tupple observations, often the x-axis is inteded to represent the independent variable (input, *cause*) and the y-axis the dependent variable (output).

![scatterplot]({{ site.url }}/images/scatterplot.PNG)

* However scatter plots can be deceiving. The two scatter plots look similar a priory. But if we use boxplots we can easily see the difference.

![boxplot]({{ site.url }}/images/boxplot.PNG)

* A boxplot is a visual simplified representation of the data distribution (rotated 90 degrees) 

![boxplot]({{ site.url }}/images/boxplot2.PNG)

* IQR stands for interquartile range, which is the distance between the upper and lower quartiles. This is the range of the middle half of the dataset.


