---
title: "R入門1"
date: "2018-10-22T18:19:22+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "r"
tags:
  - "r"
---


{{< highlight r >}}
# modulus operator (remainder of 5 divided by 2)
5 %% 2
{{< /highlight >}}

1



{{< highlight python >}}
var <- 10.
print(var^2)
print(var/3)
{{< /highlight >}}

    [1] 100
    [1] 3.333333



```R
lang.domain <- "statistics"
lang.domain <- toupper(lang.domain)
print(lang.domain)
```

    [1] "STATISTICS"



```R
# # substitutes every "i" for "I"
lang.domain <- "statistics"
gsub("i", "I", lang.domain) 
```


'statIstIcs'



```R
lang.domain <- "statistics"
substr(lang.domain, 1,4)
```


'stat'



```R
# combines character strings
lang.domain <- "statistics"
paste("R does ", lang.domain, "!!!")
```


<span style=white-space:pre-wrap>'R does  statistics !!!'</span>



```R
if ((2+2==4) && (2*2==4)) {
    print("very good")
}
```

    [1] "very good"



```R
# gsub help page
?gsub
```


```R
 # gsubの例を出す
example(gsub)
```


```R
# get the first value from a vector
our.vect <- c(8, 2, 3)
our.vect[1] = 9
print(our.vect)
```

    [1] 9 2 3



```R
our.vect <- c(8, 2, 3)
print(length(our.vect)) 
```

    [1] 3



```R
our.vect <- c(8, 2, 3)
# # 二個目の要素、一個目の要素、３個目の要素
our.vect[c(2,1,3)] 
```


<ol class=list-inline>
	<li>2</li>
	<li>8</li>
	<li>3</li>
</ol>




```R
other.vector <- 1:5
print(other.vector)
```

    [1] 1 2 3 4 5



```R
# 10と0は含まれる
another.vector <- seq(10,0, by=-2)
print(another.vector)
```

    [1] 10  8  6  4  2  0



```R
another.vector <- c(5,4,3,2,1)
# 2から4までで要素を取得
print(another.vector[2:4]) 
```

    [1] 4 3 2



```R
print(max(1:10))
print(min(1:10))
print(sum(1:10))
print(mean(1:10))
# standard  deviation
print(sd(1:10))
```

    [1] 10
    [1] 1
    [1] 55
    [1] 5.5
    [1] 3.02765



```R
messy.vector <- c(2,4,NA,4)
print(sum(messy.vector)) # NA is not allowed
print(sum(messy.vector, na.rm=TRUE))
print(sum(messy.vector, na.rm=FALSE))

```

    [1] NA
    [1] 10
    [1] NA



```R
messy.vector <- c(2,4,NA,4)
# NAであるかどうかチェック
is.na(messy.vector)
```


<ol class=list-inline>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>TRUE</li>
	<li>FALSE</li>
</ol>




```R
messy.vector <- c(2,4,NA,4)
# get a count of the number of NA
sum(is.na(messy.vector)) 
```


1



```R
our.vect <- c(8, 2, 3)
# 8だけ5より大きい
print(our.vect > 5)
print(typeof(our.vect > 5))
print(sum(our.vect>5))
```

    [1]  TRUE FALSE FALSE
    [1] "logical"
    [1] 1



```R
messy.vector <- c(2,4,NA,4)
# NAでない要素を抽出
messy.vector[!is.na(messy.vector)]
```


<ol class=list-inline>
	<li>2</li>
	<li>4</li>
	<li>4</li>
</ol>




```R
messy.vector <- c(2,4,NA,4)
# NAのところ０で埋める
messy.vector[is.na(messy.vector)] <- 0
print(messy.vector)
```

    [1] 2 4 0 4

