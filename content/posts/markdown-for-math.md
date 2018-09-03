---
title: "Hugoに使われる数式のMarkdown"
date: 2018-09-03T18:36:31+09:00
draft: false
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "math"
tags:
  - "math"
---

### 例1
```
$$
T = S((1 + \frac{E}{R})^{F} - 1)
$$
```

$$
T = S((1 + \frac{E}{R})^{F} - 1)
$$


### 例2
```
$T = S((1 + \frac{E}{R})^{F} - 1)$

```

$T = S((1 + \frac{E}{R})^{F} - 1)$


### 例3

```
$$
\begin{align}
\dot{x} & = \sigma(y-x) \newline
\dot{y} & = \rho x - y - xz \newline
\dot{z} & = -\beta z + xy
\end{align}
$$
```

$$
\begin{align}
\dot{x} & = \sigma(y-x) \newline
\dot{y} & = \rho x - y - xz \newline
\dot{z} & = -\beta z + xy
\end{align}
$$

### 例4

```
$\lim_{h \to 0} \frac{f(x+h)-f(x)}{h}$
```

$\lim_{h \to 0} \frac{f(x+h)-f(x)}{h}$


### 例5

```
$\sqrt[3]{a}$
```
$\sqrt[3]{a}$


### 例6

```
$\int_{a}^{b}f(x)dx$
```

```
$\int_{a}^{b}f(x)dx$
