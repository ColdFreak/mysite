---
title: "OpenCV ハンズオン1"
date: 2018-07-01T12:17:24+09:00
draft: false
---

画像をBGRで表示、BGRはRGB色を逆転するだけ

{{< highlight python >}}
import cv2 # OpenCVライブラリ
import numpy as np
from matplotlib import pyplot as plt # 画像を描画するライブラリ matplotlib

image_bgr = cv2.imread("../images/Lenna.png", cv2.IMREAD_COLOR)

print(image_bgr.shape) # 変なBGR色の画像が表示される
#print(image_bgr)
plt.imshow(image_bgr)
plt.show()

{{< / highlight >}}

    結果：
    (512, 512, 3)

![png](../../hands-on-opencv-1/opencv_1_0_1.png) 


画像をRGBで表示

{{< highlight python >}}
import cv2
import numpy as np
from matplotlib import pyplot as plt

image_bgr = cv2.imread("../images/Lenna.png", cv2.IMREAD_COLOR)

image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB)
#print(image_rgb.shape)
#print(image_rgb)
plt.imshow(image_rgb) # rgb色の画像が表示される
plt.show()
{{< / highlight >}}


![png](../../hands-on-opencv-1/opencv_1_1_0.png)



画像をGrayscaleで表示

{{< highlight python >}}
import cv2
import numpy as np
from matplotlib import pyplot as plt

image_bgr = cv2.imread("../images/Lenna.png", cv2.IMREAD_COLOR)

image_gray = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2GRAY)

print(image_gray.shape)
print(image_gray)
plt.imshow(image_gray, cmap='gray') # grayスケールの画像が表示される
plt.show()
{{< / highlight >}}

    結果：
    (512, 512)
    [[162 162 162 ..., 170 155 128]
     [162 162 162 ..., 170 155 128]
     [162 162 162 ..., 170 155 128]
     ...,
     [ 43  43  50 ..., 104 100  98]
     [ 44  44  55 ..., 104 105 108]
     [ 44  44  55 ..., 104 105 108]]



![png](../../hands-on-opencv-1/opencv_1_2_1.png)



Binary画像を表示

{{< highlight python >}}

import cv2
import numpy as np
from matplotlib import pyplot as plt

image_bgr = cv2.imread("../images/Lenna.png", cv2.IMREAD_COLOR)

image_gray = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2GRAY)

ret, threshd_img = cv2.threshold(image_gray, 127, 255, cv2.THRESH_BINARY)

print(ret)
plt.imshow(threshd_img, cmap='gray')
plt.show()
{{< / highlight >}}

    結果：
    127.0

![png](../../hands-on-opencv-1/opencv_1_3_2.png)



copyMakeBorderの一例

{{< highlight python >}}
import cv2
import numpy as np
from matplotlib import pyplot as plt

image_bgr = cv2.imread("../images/Lenna.png", cv2.IMREAD_COLOR)

image_gray = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2GRAY)

# top, bottom, left, right - border width in number of pixels in corresponding directions
# https://docs.opencv.org/3.4.1/d3/df2/tutorial_py_basic_ops.html
image_border = cv2.copyMakeBorder(image_gray, 58, 58, 58, 58, cv2.BORDER_REPLICATE)

plt.imshow(image_border, cmap='gray')
plt.show()
{{< / highlight >}}

![png](../../hands-on-opencv-1/opencv_1_4_1.png)


findContours, drawContoursの例

{{< highlight python >}}

import cv2
import numpy as np
from matplotlib import pyplot as plt

image_bgr = cv2.imread("../images/Lenna.png", cv2.IMREAD_COLOR)

image_gray = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2GRAY)

ret, threshd_img = cv2.threshold(image_gray, 127, 255, cv2.THRESH_BINARY)

# https://docs.opencv.org/3.4.1/d4/d73/tutorial_py_contours_begin.html
image_contour, contours, hierarchy = cv2.findContours(threshd_img, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)

# https://docs.opencv.org/3.4.1/d4/d73/tutorial_py_contours_begin.html
cv2.drawContours(image_contour, contours, -1, (111, 111, 111), 6)

plt.imshow(image_contour, cmap='gray')
plt.show()
{{< / highlight >}}

![png](../../hands-on-opencv-1/opencv_1_6_1.png)



{{<  highlight python >}}

import cv2
import numpy as np
from matplotlib import pyplot as plt

image_bgr = cv2.imread("../images/hammer.jpg", cv2.IMREAD_COLOR)

image_gray = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2GRAY)

ret, threshd_img = cv2.threshold(image_gray, 127, 255, cv2.THRESH_BINARY)

# https://docs.opencv.org/3.4.1/d4/d73/tutorial_py_contours_begin.html
image_contour, contours, hierarchy = cv2.findContours(threshd_img, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

# https://docs.opencv.org/3.4.1/d4/d73/tutorial_py_contours_begin.html
x, y, w, h = cv2.boundingRect(contours[4])
cv2.rectangle(image_contour, (x,y), (x+w, y+h), (190,190,110), 10)

plt.imshow(image_contour, cmap='gray')
plt.show()

{{< / highlight >}}

![png](../../hands-on-opencv-1/opencv_1_7_0.png)



cv2.resizeの一例

{{< highlight python >}}
import cv2
import numpy as np
from matplotlib import pyplot as plt

image_bgr = cv2.imread("../images/hammer.jpg", cv2.IMREAD_COLOR)
print(image_bgr.shape)

img_half = cv2.resize(image_bgr, None, fx=0.5, fy=0.5)

plt.imshow(img_half, cmap='gray')
plt.show()
{{< / highlight >}}

    結果：
    (410, 400, 3)



![png](../../hands-on-opencv-1/opencv_1_8_1.png)



cv2.resizeのもう一例

{{< highlight python >}}
import cv2
import numpy as np
from matplotlib import pyplot as plt

image_bgr = cv2.imread("../images/hammer.jpg", cv2.IMREAD_COLOR)

height = image_bgr.shape[0]
width = image_bgr.shape[1]


image_half = cv2.resize(image_bgr, (int(width / 2), int(height / 2) ))

print(image_half.shape)
plt.imshow(image_half, cmap='gray')
plt.show()
{{< / highlight >}}

    結果：
    (205, 200, 3)


![png](../../hands-on-opencv-1/opencv_1_9_1.png)
