---
title: "MNISTデータの可視化"
date: 2018-08-30T10:21:25+09:00
draft: true
---

`Keras`から`MNIST`データセットを取得して、表示させる。

```
$ ipython

In [1]: from keras.datasets import mnist
Using TensorFlow backend.

In [3]: (train_images, train_labels), (test_images, test_labels) = mnist.load_data()

In [5]: train_images.shape
Out[5]: (60000, 28, 28)

In [6]: train_labels.shape
Out[6]: (60000,)

In [7]: test_images.shape
Out[7]: (10000, 28, 28)

In [8]: test_labels.shape
Out[8]: (10000,)

In [3]: import matplotlib

In [4]: import matplotlib.pyplot as plt

In [5]: some_digit = train_images[0]

In [6]: some_digit_image = some_digit.reshape(28,28)

In [7]: plt.imshow(some_digit_image, cmap = matplotlib.cm.binary, interpolation="nearest")
Out[7]: <matplotlib.image.AxesImage at 0x1217a0a20>

In [8]: plt.axis("off")
Out[8]: (-0.5, 27.5, 27.5, -0.5)

In [9]: plt.show()
```
