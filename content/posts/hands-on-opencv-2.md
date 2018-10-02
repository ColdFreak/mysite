---
title: "OpenCV ハンズオン 2"
date: 2018-07-01T12:38:07+09:00
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "computer vision"
tags:
  - "opencv"
---


下の四文字を分割するタスク


{{< highlight python >}}
import cv2 # OpenCVライブラリ
import numpy as np
from matplotlib import pyplot as plt # 画像を描画するライブラリ matplotlib

image_bgr = cv2.imread("../images/2A2X.png")
image_gray = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2GRAY)

plt.imshow(image_gray, cmap='gray')
plt.show()
{{< / highlight >}}

![png](../../hands-on-opencv-2/opencv_2_1_0.png)




{{< highlight python >}}
print(image_bgr.shape)
{{< / highlight >}}

    結果：
    (24, 72, 3)



{{< highlight python >}}
# https://algorithm.joho.info/image-processing/otsu-thresholding/
ret, threshd_img = cv2.threshold(image_gray, 0, 255, cv2.THRESH_BINARY_INV | cv2.THRESH_OTSU)

plt.imshow(threshd_img, cmap='gray')

plt.show()
{{< / highlight >}}


![png](../../hands-on-opencv-2/opencv_2_3_0.png)



{{< highlight python >}}
image_contour, contours, hierarchy = cv2.findContours(threshd_img.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

print(type(contours))
print(len(contours)) # 四つ輪郭
{{< / highlight >}}

    結果：
    <class 'list'>
    4



{{< highlight python >}}
letter_image_regions = []

for contour in contours:

    (x, y, w, h) = cv2.boundingRect(contour)

    letter_image_regions.append((x, y, w, h))
    
letter_image_regions = sorted(letter_image_regions, key=lambda x: x[0])
{{< / highlight >}}


{{< highlight python >}}
letter_image_regions = []
len(letter_image_regions) # x軸でソートした四つの輪郭
{{< / highlight >}}
    
    結果：
    4




{{< highlight python >}}
print(letter_image_regions[0])
print(letter_image_regions[1])
print(letter_image_regions[2])
print(letter_image_regions[3])
{{< / highlight >}}
    
    
    結果：
    (8, 9, 9, 12)
    (23, 8, 11, 13)
    (39, 6, 9, 12)
    (53, 8, 12, 12)



{{< highlight python >}}
letter_images = []

for letter_bounding_box in letter_image_regions:

    x, y, w, h = letter_bounding_box
    
    # 個々の文字を切り出す
    letter_image = image_gray[y - 2:y + h + 2, x - 2:x + w + 2]

    letter_images.append(letter_image)
{{< / highlight >}}


{{< highlight python >}}
print(type(letter_images))
print(type(letter_images[0]))
{{< / highlight >}}

    結果：
    <class 'list'>
    <class 'numpy.ndarray'>



{{< highlight python >}}
plt.imshow(letter_images[0], cmap='gray')
plt.show()
{{< / highlight >}}


![png](../../hands-on-opencv-2/opencv_2_12_0.png)




{{< highlight python >}}
plt.imshow(letter_images[1], cmap='gray')
plt.show()
{{< / highlight >}}


![png](../../hands-on-opencv-2/opencv_2_13_0.png)

{{< highlight python >}}
plt.imshow(letter_images[2], cmap='gray')
plt.show()
{{< / highlight >}}


![png](../../hands-on-opencv-2/opencv_2_14_0.png)

{{< highlight python >}}
plt.imshow(letter_images[3], cmap='gray')
plt.show()
{{< / highlight >}}

![png](../../hands-on-opencv-2/opencv_2_15_0.png)
