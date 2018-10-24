---
title: "R入門4"
date: "2018-10-24T15:26:11+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "r"
tags:
  - "r"
---



`women` is a built-in data set in R, which holds the height and weight of 15 American women from ages 30 to 39.


{{< highlight r >}}
print(head(women))
{{< / highlight >}}

      height weight
    1     58    115
    2     59    117
    3     60    120
    4     61    123
    5     62    126
    6     63    129



{{< highlight r >}}
str(women)
{{< / highlight >}}

    'data.frame':	15 obs. of  2 variables:
     $ height: num  58 59 60 61 62 63 64 65 66 67 ...
     $ weight: num  115 117 120 123 126 129 132 135 139 142 ...



{{< highlight r >}}
# １５行ある
print(nrow(women))
{{< / highlight >}}

    [1] 15



相関を求める
$cov_{xy}=\frac{\sum_{}{}(x - \bar{x})(y-\bar{y})}{(n-1)}$


{{< highlight r >}}
print(cov(women$weight, women$height))
{{< / highlight >}}

    [1] 69


`cov`関数は少し不安定で、例えば下のように、関連性は変わっていないのに、相関の大きさは変わってしまう。


{{< highlight r >}}
print(cov(women$weight, women$height*2.54))
{{< / highlight >}}

    [1] 175.26


上の不安定問題を解決するために、`Pearson's correlation coefficient `を使うことができる。


Pearson's correlation coefficient is usually denoted by r and its equation is given as follows:

$$ r = \frac{\sum_{}{} (x-\bar{x})(y-\bar{y})}{(n-1)s_xx_y}$$

which is the covariance divided by the product of the two variables' standard deviation.



{{< highlight r >}}
print(cor(women$weight, women$height))
{{< / highlight >}}

    [1] 0.9954948



{{< highlight r >}}
print(cor(women$weight, women$height*2.54))
{{< / highlight >}}

    [1] 0.9954948


If there is a strong relationship between two variables, but the relationship
is not linear, it cannot be represented accurately by Pearson's r.




{{< highlight r >}}
xs <- 1:100
print(cor(xs, xs+100)) # OK 
print(cor(xs, xs^3)) # cubic relationship NO!!!
{{< / highlight >}}

    [1] 1
    [1] 0.917552


Pearson's r assumes a linear relationship between two variables. There are, however, other correlation coefficients that are more tolerant of non-linear relationships. Probably the most common of these is Spearman's rank coefficient, also called `Spearman's rho`. Spearman's rho is calculated by taking the Pearson correlation not of the values, but of their ranks.

$$ \rho = \frac{6\sum_{}{}d_i^2}{n(n^2 -1)}$$


$\rho$ = Spearman rank correlation

$d_i$ = the difference between the ranks of corresponding variables

$n$ = number of observations


{{< highlight r >}}
xs <- 1:100
print(cor(xs, xs+100, method="spearman")) # OK 
print(cor(xs, xs^3, method="spearman")) # OK
{{< / highlight >}}

    [1] 1
    [1] 1


