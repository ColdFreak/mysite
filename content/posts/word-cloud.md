---
title: "Word Cloud"
date: "2018-09-30T11:22:50+09:00"
draft: false
readmore: true # Show "Read more" button in list if true
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "wordcloud"
tags:
  - "wordcloud"
---

[ワインのレビュー](https://github.com/davestroud/Wine/blob/master/winemag-data-130k-v2.csv)を使って、word cloudを作成する。

レビュー全体のword cloudを作る

{{< highlight python >}}
import pandas as pd
from wordcloud import WordCloud

import matplotlib.pyplot as plt
% matplotlib inline

df = pd.read_csv("data/winemag-data-130k-v2.csv", index_col=0)

# 1個目のレビューのword cloud
text = df.description
wordcloud = WordCloud.generate(text)

plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.show()
{{< / highlight >}}

![png](../../word-cloud/1.png) 

Fraceのレビューを作る


{{< highlight python >}}
frace_description = df[df.country == "France"].description
text = ' '.join(frace_description)
wordcloud = WordCloud().generate(text)

# Display the generated image:
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.show()

{{< / highlight >}}

![png](../../word-cloud/2.png) 


国別のword cloudを作って、レビューも違ってくることがわかる。
