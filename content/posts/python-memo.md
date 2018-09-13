---
title: "(WIP)Pythonのメモ"
date: "2018-09-13T13:39:40+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - ""
  - "python"
tags:
  - "python"
---

#### 分数を扱う

```
In [1]: from fractions import Fraction

In [2]: f = Fraction(3,4)

In [3]: f
Out[3]: Fraction(3, 4)

In [4]: f + 0.26
Out[4]: 1.01
```

#### 複素数

```
In [5]: a = complex(2,3)

In [6]: a
Out[6]: (2+3j)

In [7]: a.real ## 実部
Out[7]: 2.0

In [8]: a.imag ## 虚部
Out[8]: 3.0

```
