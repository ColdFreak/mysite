---
title: "MNISTデータの可視化"
date: 2018-08-30T10:21:25+09:00
draft: true
---

scikit-learnからMNISTデータを取得

```python
from sklearn.datasets import fetch_mldata
In [1]: import pandas as pd
In [1]: import matplotlib.pyplot as plt
In [2]: from sklearn.datasets import fetch_mldata

In [4]: mnist = fetch_mldata("MNIST original")

In [5]: mnist.data
Out[6]:
array([[0, 0, 0, ..., 0, 0, 0],
       [0, 0, 0, ..., 0, 0, 0],
       [0, 0, 0, ..., 0, 0, 0],
       ...,
       [0, 0, 0, ..., 0, 0, 0],
       [0, 0, 0, ..., 0, 0, 0],
       [0, 0, 0, ..., 0, 0, 0]], dtype=uint8)

In [7]: mnist.data.shape
Out[7]: (70000, 784)

In [8]: mnist.target
Out[8]: array([0., 0., 0., ..., 9., 9., 9.])

In [9]: mnist.target.shape
Out[9]: (70000,)


In [11]: X = mnist.data / 255.0
    ...:

In [12]: y = mnist.target

In [13]: print(X.shape, y.shape)
(70000, 784) (70000,)

In [14]: feat_cols = [ 'pixel'+str(i) for i in range(X.shape[1]) ]

In [22]: df['label'] = y

In [30]: df['label'] = df['label'].apply(lambda i: str(i))

In [32]: X, y = None, None

In [36]: print('Size of the dataframe: {}'.format(df.shape))
Size of the dataframe: (70000, 785)

In [39]: rndperm = np.random.permutation(df.shape[0])

In [40]: rndperm
Out[40]: array([64330, 62132, 26866, ..., 63046,  3155, 65599])
```

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
