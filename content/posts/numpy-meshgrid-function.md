---
title: "NumpyのMeshgrid関数"
date: "2018-10-02T07:47:17+09:00"
draft: false
description: "NumpyのMshagrid関数の例"
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "numpy"
tags:
  - "numpy"
---


`numpy.meshgrid`は以下のようにNumpy配列を生成します。


![png](../../numpy-meshgrid-function/1.png) 

画像などの座標位置とか3次元グラフのX,Y軸の座標とかのIndexに使われるmeshgridを生成する。
つまり(0,0),(0,1)....(1,0),(1,1)....のようなデータを作るのにも使われます。

一つの例$Z =  X * np.exp(-X**2-Y**2) -1$を通して、meshgridのイメージをつかむ。

{{< highlight python >}}
import numpy as np

x = np.mgrid[-2:2:0.2] # [-2, -1.8, -1.6, ... , 1.6, 1.8]
y = np.mgrid[-2:2:0.2]
X, Y = np.meshgrid(x, y)
Z =  X * np.exp(-X**2-Y**2) -1

import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
fig = plt.figure(figsize=(20,15))

ax = fig.gca(projection='3d')
plt.xlabel('X')
plt.ylabel("Y")
#surf = ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap=cm.jet,linewidth=0.1, antialiased=False)
surf = ax.plot_surface(X, Y, Z, cmap=cm.jet)
plt.show()

{{< / highlight >}}


![png](../../numpy-meshgrid-function/2.png) 
