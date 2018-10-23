---
title: "自分のブログをGoogle Search Consoleに登録"
date: "2018-10-24T08:44:41+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "search engine"
tags:
  - "search engine"
---

最近自分のHugoで作られたブログがGoogle検索エンジンに検索できないことが気づいて、それを解決しました。

まずwww.google.comにアクセスして、`site:https://pizi.netlify.com`で検索する。もちろん検索結果はないですが、下のような情報が出ていました。

```
Try Google Search Console
www.google.com/webmasters/
Do you own https/pizinetlify.com? Get indexing and ranking data from Google.
```

調べてみると、Google Search Consoleにアクセスして`sitemap.xml`をsubmitする必要があります。

私の場合`sitemap.xml`は`https://pizi.netlify.com/sitemap.xml`にあります。Hugoが自動的に作られています。


![png](../../verify-site-in-google-search-console/1.png) 


Submitして、しばらく待つと`google.com`に`site:https://pizi.netlify.com`は検索できるようになりました。 
