---
title: "Jaccard係数"
date: "2018-09-29T20:40:21+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "jaccardsimilarity"
tags:
  - "jaccardsimilarity"
---

Jaccard係数は2つの集合に含まれている要素のうち共通要素が占める割合を表しています。
Jaccard係数が大きいほど2つの集合の類似度は高い

$$
J(A, B) = \frac{|A \bigcap B|}{|A \bigcup B|}
$$

Rで計算してみます

{{< highlight r >}}
> library('clusteval')
> vec1 = c(1,1,1,0,0,0,0,0,0,0,0,0)
> vec2 = c(0,0,1,1,1,1,1,0,1,0,0,0)
> cluster_similarity(vec1, vec2, similarity = "jaccard")
[1] 0.3269231
{{< / highlight  >}}
