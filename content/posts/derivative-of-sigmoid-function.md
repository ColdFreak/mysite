---
title: "シグモイド関数の微分"
date: "2018-09-12T17:23:22+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "math"
tags:
  - "math"
---


シグモイド関数(Sigmoid function)は $f(x) = \frac{1}{1+e^{-ax}}  (a > 0) $ の形をしている関数です。下の図で表しています。


![png](../../derivative-of-sigmoid-function/1.png) 

値は(0, 1)の間にあるので、よく確率として使われています。例えば 0.7だったら70%の確率という意味をします。

ニューラルネットワークの中で、シグモイド関数はよくTrue/Falseの判定に使われています。例えば画像が猫なのか、猫じゃないなのかを判断する場合、0.7だったら、猫であると判定します。

このようなニューラルネットワークを実装する際に逆伝播(Back propagation)を実装する必要になります。その中の一部として、シグモイド関数の微分を求めることになります。

<img width="770" alt="スクリーンショット 2018-05-29 17.44.47.png" src="https://idcf-developers.qiita.com/files/84d280d6-688b-e807-c7ef-e5c17c6e580a.png">



シグモイド関数の微分は 下の形をしています。これを方程式を使えば、Numpyでかなり簡単に実装できるようなります。

$$
f(x) = \frac{1}{1+e^{-ax}}  (a > 0) \\\ 
f(x)'  = af(x)(1−f(x))
$$
上の方程式を導いてみると

$$
(\frac{1}{1+e^{-ax}})' = \\\ 
\frac{-1} {(1+e^{-ax})^2}   (1+e^{-ax}) = \\\ 
\frac{-1} {(1+e^{-ax})^2}  (e^{-ax})(-ax)' = \\\ 
\frac{-1} {(1+e^{-ax})^2}  (e^{-ax})(-a) = \\\ 
\frac{ae^{-ax}} {(1+e^{-ax})^2} = \\\
\frac{a}{(1+e^{-ax})}  \frac{e^{-ax}}{(1+e^{-ax})}  = \\\
\frac{a}{(1+e^{-ax})} \frac{1+ e^{-ax} -1 }{(1+e^{-ax})} = \\\
af(x)(1-f(x))
$$

$a = 1$の時に $f(x)' = f(x)(1-f(x))  $　になります。
