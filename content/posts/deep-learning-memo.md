---
title: "(WIP)統計&機械学習&画像関連のメモ"
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

### Machine Learning

- Except on very simple problems, `RMSProp` optimizer almost always performs much better than `AdaGrad`, It also generally performs better than Momentum optimization and Nesterov Accelerated Gradients. In fact, it was the preferred optimization algorithm of many researchers until Adam optimization came around.


- You can actually use a linear model to fit nonlinear data. A simple  way to do this is to add powers of each feature as new features, then train a linear model on this extended set of features. This technique is called `Polymonial Regression`


### Statistic

- `Trimmed mean` = $\bar{x} = \frac{\sum_{i=p+1}^{n-p}x_i}{n-2p}$. A variation of the mean is a trimmed mean, which you calculate by dropping a fixed number of sorted values at each end and then taking an average of the
remaining values.

- `Standardization`, also called *normalization*, puts all variables on similar scales by subtracting the mean and dividing by the standard deviation. $Z = \frac{X-\bar{X}}{S}$

- In the regression setting, the most commonly-used measure is the `mean squared error`(MSE), given by $MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{f}(x_i))^2$

- The formula for `standard deviation` (denoted by s) is as follows, where *n* equals the number of values in the data set, each x represents a number in the data set, and $\bar{x}$ is the average of all the data: $s = \sqrt{\sum{\frac{(x-\bar{x})^2}{n-1}}}$ . If your data have the form of normal distribution, about 95% of the data lie within two standard deviations of the mean. This result is called the empirical rule, or the 68–95–99.7% rule. 
- The `Z-distribution` is a normal distribution with mean zero and standard deviation 1.

- All hypothesis tests ultimately use a p-value to weigh the strength of the evidence.or example, suppose a pizza place claims their delivery times are 30 minutes or less on average but you think it’s more than that. You conduct a `hypothesis test` because you believe the `null hypothesis`, $H_o$ , that the mean delivery time is 30 minutes max, is incorrect. Your alternative hypothesis ($H_a$) is that the mean time is greater than 30 minutes. You randomly sample some delivery times and run the data through the hypothesis test, and your p-value turns out to be 0.001, which is much less than 0.05. You conclude that the pizza place is wrong;


- The `Empirical Rule` says that if a population has a normal distribution with population mean $\mu$ and standard deviation $\sigma$, then:
 - About 68% of the values lie within 1 standard deviation of the mean (or between the mean minus 1 times the standard deviation, and the mean plus 1 times the standard deviation). In statistical notation, this is represented as $\mu \pm 1\sigma$ .
 - About 95% of the values lie within 2 standard deviations of the mean (or between the mean minus 2 times the standard deviation, and the mean plus 2 times the standard deviation). The statistical notation for this is $\mu \pm 2\sigma$ .
 - About 99.7% of the values lie within 3 standard deviations of the mean (or between the mean minus 3 times the standard deviation and the mean plus 3 times the standard deviation). Statisticians use the following notation to represent this: $\mu \pm 3\sigma$ .


- `ANOVA` is the acronym for analysis of variance. You use ANOVA in situations where you want to compare the means of more than two populations. 

- You can run into situations where the data being studied isn’t quantitative, but rather categorical — that is, the data represent categories, not measurements or counts. To study relationships in categorical data, you use a `Chi-square test` for independence. 

- The `standard  error` of the mean is designated as: $\sigma_M$. It is the standard deviation of the sampling distribution of the mean. The formula for the standard error of the mean is: $\sigma_M = \frac{\sigma}{\sqrt{N}}$ . where $\sigma$ is the standard deviation of the original distribution and N is the sample size .

- The  `t-distribution` is typically used to study the mean of a population, rather than to study the individuals within a population. For example, to estimate the average price of all the new homes in California. `t-distribution` is shorter and flatter than the `Z-distribution`. Its standard deviation is proportionally larger compared to the Z

- [`The Binomial Distribution`](https://www.mathsisfun.com/data/binomial-distribution.html) Example 1: Toss a fair coin 9 times ... what is the chance of getting 5 Heads? It's 
$$C_{9}^{5} = \frac{5!}{5!(9-5)!} = \frac{126}{512}$$ . 
Example 2: You sell sandwiches. 70% of people choose chicken, the rest choose something else. What is the probability of selling 2 chicken sandwiches to the next 3 customers? It's 

$$
p^k * (1-p)^{n-k}*C_{n}^{k} = \\\ 
$$

$$
0.7^5 * (1-0.7)^{9-5}*C_{9}^{5}
$$

- We measure the heights of 40 randomly chosen men, and get a mean height of 175cm, We also know the standard deviation of men's heights is 20cm, The [`95% Confidence Interval`](https://www.mathsisfun.com/data/confidence-interval.html) is $175cm \pm 6.2cm$ . A Confidence Interval is a range of values we are fairly sure our true value lies in. The 95% says that **95% of experiments like we just did** will include the true mean, but 5% won't. **For 95% the Z value** is *1.960*, use that Z in this formula for the Confidence Interval 
  $$\bar{X} \pm Z\frac{s}{\sqrt{n}} = $$
  $$175 \pm 1.960 * \frac{20}{\sqrt{40}}$$
  So there is a 1-in-20 chance (5%) that our Confidence Interval does NOT include the true mean. The value after the $\pm$ is called the margin of error
  
- One important way to assess how well the model fits is to use a statistic called the `coefficient of determination`, or $R^2$. This statistic takes the value of the correlation, $R$, and squares it to give you a percentage.
$$R^2=1- \frac{\sum_{}{}(y_i - f_i)^2}{\sum{}{}(y_i - \bar{y})^2}$$
$y_i$ is observed data and $f_i$ is the predicted data. $\bar{y}$ is the mean of the observed data. [link](https://ja.wikipedia.org/wiki/%E6%B1%BA%E5%AE%9A%E4%BF%82%E6%95%B0)

- `Multicolinearity` is a term you use if two x variables are highly correlated. Not only is it redundant to include both related variables in the multiple regression model, but it’s also problematic. The bottom line is this  :**If two x variables are significantly correlated, only include one of them in the regression model, not both**. If you include both, the computer won’t know what numbers to give as coefficients for each of the two variables because they share their contribution to determining the value of y. Multicolinearity can really mess up the model- fitting process and give answers that are inconsistent and often not repeatable in subsequent studies.

- The coefficient of an x variable in a multiple regression model is the amount by which y changes if that x variable increases by one unit and the values of all other x variables in the model don’t change. So basically, you’re looking at the marginal contribution of each x variable when you hold the other vari- ables in the model constant.




### Image processing

- The `aspect ratio` of an image describes the proportional relationship between its width and its height. It is commonly expressed as two numbers separated by a colon, as in 16:9. For an x:y aspect ratio

### Others

- [`Moore's law`](https://en.wikipedia.org/wiki/Moore%27s_law) is the observation that the number of transistors in a dense integrated circuit doubles about every two years.

- In computer science, `Monte Carlo tree search (MCTS)` is a heuristic search algorithm for some kinds of decision processes, most notably those employed in game play. Two leading examples of Monte Carlo tree search are the computer game Total War: Rome II's implementation in their high level campaign AI and recent computer Go programs. [link](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search)
