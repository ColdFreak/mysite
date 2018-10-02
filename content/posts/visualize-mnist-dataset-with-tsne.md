---
title: "MNISTデータセットをT-SNEで可視化"
date: "2018-09-28T10:05:34+09:00"
draft: false
description: "Example article description"
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "deep learning"
tags:
  - "t-sne"
---


scikit learnからMNISTデータセットを取得して、T-SNEを適応した後に描画する。

{{< highlight python>}}
from sklearn.datasets import fetch_mldata

mnist = fetch_mldata('MNIST original')

type(mnist)
{{< / highlight >}}

```
sklearn.utils.Bunch
```

ランダムに`10000`のデータを選択する

{{< highlight python>}}
import numpy as np
np.random.seed(42)
m = 10000
idx = np.random.permutation(60000)[:m]
idx
{{< / highlight >}}

```
array([12628, 37730, 39991, ...,  3256, 14474, 41816])
```

Yはtargetで、1-9までの数値に当てはまる。

{{< highlight python>}}
X = mnist['data'][idx]
Y = mnist['target'][idx]
{{< / highlight >}}



MNISTを2次元のデータに変換するのは２分ぐらいかかります。

{{< highlight python>}}
from sklearn.manifold import TSNE

tsne = TSNE(n_components=2, random_state=42)
X_reduced  = tsne.fit_transform(X)
{{< / highlight >}}


変換したテータを描画して、クラスタになっていることを確認する。

{{< highlight python>}}
%matplotlib inline
import matplotlib
import matplotlib.pyplot as plt

plt.figure(figsize=(13,10))
plt.scatter(X_reduced[:, 0], X_reduced[:, 1], c=Y, cmap="jet")
plt.colorbar()
plt.show()
{{< / highlight >}}

![png](../../visualize-mnist-dataset-with-tsne/1.png)
