---
title: "(WIP)統計や機械学習関連のメモ"
date: 2018-08-30T10:21:25+09:00
draft: false
authorbox: true # Optional, enable authorbox for specific post
toc: false # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "math"
tags:
  - "math"

---


Except on very simple problems, `RMSProp` optimizer almost always performs much better than `AdaGrad`, It also generally performs better than Momentum optimization and Nesterov Accelerated Gradients. In fact, it was the preferred optimization algorithm of many researchers until Adam optimization came around.


You can actually use a linear model to fit nonlinear data. A simple  way to do this is to add powers of each feature as new features, then train a linear model on this extended set of features. This technique is called `Polymonial Regression`


`Trimmed mean` = $\bar{x} = \frac{\sum_{i=p+1}^{n-p}x_i}{n-2p}$. A variation of the mean is a trimmed mean, which you calculate by dropping a fixed number of sorted values at each end and then taking an average of the
remaining values.

`Standardization`, also called *normalization*, puts all variables on similar scales by subtracting the mean and dividing by the standard deviation. $Z = \frac{X-\bar{X}}{S}$

In the regression setting, the most commonly-used measure is the `mean squared error`(MSE), given by $MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{f}(x_i))^2$

The formula for `standard deviation` (denoted by s) is as follows, where *n* equals the number of values in the data set, each x represents a number in the data set, and $\bar{x}$ is the average of all the data: $s = \sqrt{\sum{\frac{(x-\bar{x})^2}{n-1}}}$ . If your data have the form of normal distribution, about 95% of the data lie within two standard deviations of the mean. This result is called the empirical rule, or the 68–95–99.7% rule. 
