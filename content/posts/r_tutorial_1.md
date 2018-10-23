---
title: "R入門1"
date: "2018-10-23T10:39:47+09:00"
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


Rの入門1

{{< highlight r >}}
# modulus operator (remainder of 5 divided by 2)
print(5 %% 2)
{{< / highlight >}}

    [1] 1



{{< highlight r >}}
var <- 10.
print(var^2)
print(var/3)
{{< / highlight >}}

    [1] 100
    [1] 3.333333



{{< highlight r >}}
lang.domain <- "statistics"
lang.domain <- toupper(lang.domain)
print(lang.domain)
{{< / highlight >}}

    [1] "STATISTICS"



{{< highlight r >}}
# # substitutes every "i" for "I"
lang.domain <- "statistics"
print(gsub("i", "I", lang.domain) )
{{< / highlight >}}

    [1] "statIstIcs"



{{< highlight r >}}
lang.domain <- "statistics"
print(substr(lang.domain, 1,4))
{{< / highlight >}}

    [1] "stat"



{{< highlight r >}}
# combines character strings
lang.domain <- "statistics"
print(paste("R does ", lang.domain, "!!!"))
{{< / highlight >}}

    [1] "R does  statistics !!!"



{{< highlight r >}}
if ((2+2==4) && (2*2==4)) {
    print("very good")
}
{{< / highlight >}}

    [1] "very good"



{{< highlight r >}}
# gsub help page
?gsub
{{< / highlight >}}


{{< highlight r >}}
 # gsubの例を出す
example(gsub)
{{< / highlight >}}


{{< highlight r >}}
# get the first value from a vector
our.vect <- c(8, 2, 3)
our.vect[1] = 9
print(our.vect)
{{< / highlight >}}

    [1] 9 2 3



{{< highlight r >}}
our.vect <- c(8, 2, 3)
print(length(our.vect)) 
{{< / highlight >}}

    [1] 3



{{< highlight r >}}
our.vect <- c(8, 2, 3)
# # 二個目の要素、一個目の要素、３個目の要素
print(our.vect[c(2,1,3)] )
{{< / highlight >}}

    [1] 2 8 3



{{< highlight r >}}
other.vector <- 1:5
print(other.vector)
{{< / highlight >}}

    [1] 1 2 3 4 5



{{< highlight r >}}
# 10と0は含まれる
another.vector <- seq(10,0, by=-2)
print(another.vector)
{{< / highlight >}}

    [1] 10  8  6  4  2  0



{{< highlight r >}}
{{< highlight r >}}{{< highlight r >}}
another.vector <- c(5,4,3,2,1)
# 2から4までで要素を取得
print(another.vector[2:4]) 
{{< / highlight >}}

    [1] 4 3 2



{{< highlight r >}}
print(max(1:10))
print(min(1:10))
print(sum(1:10))
print(mean(1:10))
# standard  deviation
print(sd(1:10))
{{< / highlight >}}

    [1] 10
    [1] 1
    [1] 55
    [1] 5.5
    [1] 3.02765



{{< highlight r >}}
messy.vector <- c(2,4,NA,4)
print(sum(messy.vector)) # NA is not allowed
print(sum(messy.vector, na.rm=TRUE))
print(sum(messy.vector, na.rm=FALSE))

{{< / highlight >}}

    [1] NA
    [1] 10
    [1] NA



{{< highlight r >}}
messy.vector <- c(2,4,NA,4)
# NAであるかどうかチェック
print(is.na(messy.vector))
{{< / highlight >}}

    [1] FALSE FALSE  TRUE FALSE



{{< highlight r >}}
messy.vector <- c(2,4,NA,4)
# get a count of the number of NA
print(sum(is.na(messy.vector)) )
{{< / highlight >}}

    [1] 1



{{< highlight r >}}
our.vect <- c(8, 2, 3)
# 8だけ5より大きい
print(our.vect > 5)
print(typeof(our.vect > 5))
print(sum(our.vect>5))
{{< / highlight >}}

    [1]  TRUE FALSE FALSE
    [1] "logical"
    [1] 1



{{< highlight r >}}{{< highlight r >}}
messy.vector <- c(2,4,NA,4)
# NAでない要素を抽出
print(messy.vector[!is.na(messy.vector)])
{{< / highlight >}}

    [1] 2 4 4



{{< highlight r >}}
messy.vector <- c(2,4,NA,4)
# NAのところ０で埋める
messy.vector[is.na(messy.vector)] <- 0
print(messy.vector)
{{< / highlight >}}

    [1] 2 4 0 4



{{< highlight r >}}
# extract every other digit 
our.vect <- c(8, 2, 3)
print(our.vect[c(TRUE, FALSE)])
{{< / highlight >}}

    [1] 8 3



{{< highlight r >}}
# 関数の作り方
is.divisible.by <- function(large.number, smaller.number) {
    if (large.number %% smaller.number != 0)
        # FALSEの括弧が必要
        return (FALSE) 
    return (TRUE)
}

is.even <- function(num) {
    is.divisible.by(num, 2)
}

our.vect <- c(8, 2, 3, 6)
print(sapply(our.vect, is.even))

# sapplyの第二の引数直接関数を実装する
print( sapply(our.vect, function(num){is.divisible.by(num, 3)})  )
{{< / highlight >}}

    [1]  TRUE  TRUE FALSE  TRUE
    [1] FALSE FALSE  TRUE  TRUE



{{< highlight r >}}
# 上で定義した関数を利用して、要素を抽出
our.vect <- c(8, 2, 3, 6)
where.even <- sapply(our.vect, is.even)
where.div.3 <- sapply(our.vect, function(num){
    is.divisible.by(num, 3)
})

our.vect[where.even & where.div.3]
{{< / highlight >}}


6



{{< highlight r >}}
# R has the matrix data structures
a.matrix <- matrix(c(2,3,4,5))
print(a.matrix)
{{< / highlight >}}

         [,1]
    [1,]    2
    [2,]    3
    [3,]    4
    [4,]    5



{{< highlight r >}}
a.matrix <- matrix(c(2,3,4,5), ncol=2)
print(a.matrix)
{{< / highlight >}}

         [,1] [,2]
    [1,]    2    4
    [2,]    3    5



{{< highlight r >}}
# bind two vectors
# colume bind 
a2.matrix <- cbind(c(2,3,4), c(4,5,6))
print(a2.matrix)
{{< / highlight >}}

         [,1] [,2]
    [1,]    2    4
    [2,]    3    5
    [3,]    4    6



{{< highlight r >}}
# row bind
a3.matrix <- rbind(c(2,3,4), c(4,5,6))
print(a3.matrix)
{{< / highlight >}}

         [,1] [,2] [,3]
    [1,]    2    3    4
    [2,]    4    5    6



{{< highlight r >}}
a3.matrix <- rbind(c(2,3,4), c(4,5,6))
# matrix transposition
print(t(a3.matrix))
{{< / highlight >}}

         [,1] [,2]
    [1,]    2    4
    [2,]    3    5
    [3,]    4    6



{{< highlight r >}}
a3.matrix <- rbind(c(2,3,4), c(4,5,6))
print(a3.matrix)

# if vectors have sapply(), then matrices have apply()
# 1 for row
print(apply(a3.matrix, 1, sum) ) 
# 2 for  colume
print(apply(a3.matrix, 2, mean) ) 
{{< / highlight >}}

         [,1] [,2] [,3]
    [1,]    2    3    4
    [2,]    4    5    6
    [1]  9 15
    [1] 3 4 5


