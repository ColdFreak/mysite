---
title: "Hello World With Keras"
date: "2018-09-17T17:27:24+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "keras"
tags:
  - "keras"
---

KerasでMNIST手書き数字を分類する

```
In [1]: from keras.datasets import mnist
Using TensorFlow backend.
(
In [2]: (train_images, train_labels), (test_images, test_labels) = mnist.load_data()

In [3]: from keras import models

In [4]: from keras import layers

In [5]: network = models.Sequential()

# 最初の隠れ層は512を設定する, 活性化関数をReluを設定する
In [6]: network.add(layers.Dense(512, activation="relu", input_shape=(28*28,)))

# 出力10個、0-9まで１０個の出力あるから
In [7]: network.add(layers.Dense(10, activation="softmax"))

In [9]: network.compile(optimizer="rmsprop", loss="categorical_crossentropy", metrics=["accuracy"])

In [10]: train_images = train_images.reshape((60000, 28*28))

# [0,1]に収まるように正規化する
In [11]: train_images = train_images.astype("float32") / 255

In [12]: test_images = test_images.reshape((10000, 28*28))

In [13]: test_images = test_images.astype("float32") / 255

In [14]: from keras.utils import to_categorical

# 整数のクラスベクトルから2値クラスの行列への変換します．
# https://keras.io/ja/utils/#to_categorical
In [15]: train_labels = to_categorical(train_labels)

In [16]: test_labels = to_categorical(test_labels)

# fit the model to its training data
# the network iterate on the training data in mini-batches of 128 samples, 5 times over
# each iteration over all the training data is called an epoch
In [17]: network.fit(train_images, train_labels, epochs=5, batch_size=128)
Epoch 1/5
2018-09-17 17:54:52.424286: I tensorflow/core/platform/cpu_feature_guard.cc:141] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
60000/60000 [==============================] - 2s 33us/step - loss: 0.2556 - acc: 0.9257
Epoch 2/5
60000/60000 [==============================] - 2s 30us/step - loss: 0.1026 - acc: 0.9700
Epoch 3/5
60000/60000 [==============================] - 2s 30us/step - loss: 0.0664 - acc: 0.9797
Epoch 4/5
60000/60000 [==============================] - 2s 30us/step - loss: 0.0493 - acc: 0.9853
Epoch 5/5
60000/60000 [==============================] - 2s 30us/step - loss: 0.0369 - acc: 0.9890
Out[17]: <keras.callbacks.History at 0x104f08d30>

In [18]: test_loss, test_acc = network.evaluate(test_images, test_labels)
10000/10000 [==============================] - 0s 22us/step

In [19]: print('test_acc:', test_acc)
test_acc: 0.9806
```
