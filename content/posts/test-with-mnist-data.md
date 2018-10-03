---
title: "MNISTデータセットでテスト"
date: "2018-10-03T16:25:52+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "machine learning"
tags:
  - "scikit learning"
---

Hands on Machine Learning の第3章で、モデルの精度を評価する際にいろんな方法があるが、分類問題には適していない評価方法もあって、適している評価方法もある。

{{< highlight python >}}
from sklearn.datasets import fetch_mldata
import numpy as np

# $HOME/scikit_learn_dataディレクトリにデータがダウンロードされる
mnist = fetch_mldata('MNIST original') 
X, y = mnist["data"], mnist["target"]

X_train, X_test, y_train, y_test = X[:60000], X[60000:], y[:60000], y[60000:]

shuffle_index = np.random.permutation(60000)
X_train, y_train = X_train[shuffle_index], y_train[shuffle_index]
{{< /highlight >}}


{{< highlight python >}}
y_train_5 = (y_train == 5)
y_test_5 = (y_test == 5)

print("y_train_5: {}\ny_test_5: {}\ny_train_5.shape: {}".format(y_train_5, y_test_5, y_train_5.shape))
{{< /highlight >}}

```
y_train_5: [False False False ... False False False]
y_test_5: [False False False ... False False False]
y_train_5.shape: (60000,)

```

Scikit-LearnのSGDClassifierを使って、学習をせせる。SGDClassifierは大きいデータセットを扱うことが容易で、訓練データを一個ずつで学習する。

{{< highlight python >}}
from sklearn.linear_model import SGDClassifier
sgd_clf = SGDClassifier(random_state = 42, max_iter=5, tol=None)
sgd_clf.fit(X_train, y_train_5)

{{< /highlight >}}

```
SGDClassifier(alpha=0.0001, average=False, class_weight=None, epsilon=0.1,
       eta0=0.0, fit_intercept=True, l1_ratio=0.15,
       learning_rate='optimal', loss='hinge', max_iter=5, n_iter=None,
       n_jobs=1, penalty='l2', power_t=0.5, random_state=42, shuffle=True,
       tol=None, verbose=0, warm_start=False)

```


{{< highlight python >}}
from sklearn.model_selection import cross_val_score
# accuracyは分類問題にいい評価指標とは言えない
cross_val_score(sgd_clf, X_train, y_train_5, cv=3, scoring="accuracy")

{{< /highlight >}}

```
array([0.9649 , 0.94305, 0.963  ])
```

{{< highlight python >}}
from sklearn.base import BaseEstimator
class Never5Classifier(BaseEstimator):
    def fit(self, X, y=None):
        pass
    def predict(self, X):
        return np.zeros((len(X), 1), dtype=bool)
{{< /highlight >}}


何も学習しなくて、ただ5ではないと予測するだけでも90%以上の正解率。だからこのaccuracyという指標はよくない.
なぜかというと全部の教師データの中で5は10%しなかいから、5ではないと予測して、90%の正解率あるのもおかしくない。
{{< highlight python >}}

never_5_clf = Never5Classifier()
cross_val_score(never_5_clf, X_train, y_train_5, cv=5, scoring="accuracy")
{{< /highlight >}}

```
array([0.91241667, 0.91033333, 0.9095    , 0.90966667, 0.90633333])
```


分類問題を解決する際の精度評価は accuracyというより `confusion matrix`のほうがいい。

{{< highlight python >}}
from sklearn.metrics import confusion_matrix
from sklearn.model_selection import cross_val_predict

y_train_pred = cross_val_predict(sgd_clf, X_train, y_train_5, cv=3)
confusion_matrix(y_train_5, y_train_pred)
{{< /highlight >}}

```
array([[52809,  1770],
       [  811,  4610]])
```

{{< highlight python >}}
from sklearn.metrics import precision_score, recall_score
print("precision score: ", precision_score(y_train_5, y_train_pred )  )# 4610/(4610+1770)
print("recall score: ", recall_score(y_train_5, y_train_pred )  ) # 4610/(4610+811)
{{< /highlight >}}

```
precision score:  0.7225705329153606
recall score:  0.8503966057922893
```


`precision score` と `recall score`を組み合わせて $F_1$ scoreを使うこともできる

$F_1 = \frac{2}{\frac{1}{precision}+\frac{1}{recall}} $

precision と recallはほぼ同じ範囲内であればF1 scoreを使うといい。

{{< highlight python >}}
from sklearn.metrics import f1_score
f1_score(y_train_5, y_train_pred)
{{< /highlight >}}

```
0.781289721210067
```

$F_1$ scoreはやはりそんな高い精度出していないことがわかった。

