---
title: "Rを使って、コイン投げをシミュレート"
date: "2018-10-04T17:18:55+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "programming"
  - "statistic"
  - "r"
tags:
  - "r"
---

500回コインを投げるとします。毎回表と裏のどれかをシミュレートする。
`sample`関数はTRUEになっていますが、これは毎回0と1の中からランダムに選ぶという意味、１回選んだら、それを除いて、残りから選ぶではない。 [link](https://web.ma.utexas.edu/users/parker/sampling/repl.htm)

{{< highlight r >}}
> # 表が出る確率0.5
> pHeads = 0.5 

> # 0(裏)と1(表)二種類の可能性しかない。裏がでる確率が 1-pHeads
> flipSequence = sample( x=c(0,1), prob=c(1-pHeads,pHeads), size=N, replace=TRUE) 

> # 実際にシミュレートした結果
> flipSequence
  [1] 1 0 0 1 1 0 1 0 1 0 1 0 0 1 0 1 1 0 1 0 0 0 0 1 1 1 1
 [28] 0 1 0 1 0 1 0 0 0 1 0 0 0 0 0 1 0 1 1 1 0 0 0 0 0 0 1
 [55] 1 0 1 0 0 0 0 1 1 1 1 0 0 0 0 1 1 0 0 0 1 0 0 1 0 0 1
 [82] 0 1 0 0 0 1 1 0 1 1 1 1 0 1 0 1 1 0 0 0 0 1 1 0 1 0 1
 ...
 ...


> # 1回目コイン投げると1回表出た、14回目なげると10回表出た意味
> r = cumsum(filpSequence)
> r
  [1]   1   2   3   4   5   5   5   5   6   7   8   9   9
 [14]  10  11  12  12  13  14  14  15  16  16  16  17  17
 [27]  17  17  17  18  18  18  19  19  20  20  21  21  21
 ...
 ...

> n = 1:N 

> # したのデータでは 235回目コイン投げたら表出る確率は23%, 496回目コインなげると表出る確率は49.8%になっている. なかり0.5に近づいた 
> runProp = r / N
> runProp
  [1] 0.002 0.004 0.006 0.008 0.010 0.010 0.010 0.010 0.012
 [10] 0.014 0.016 0.018 0.018 0.020 0.022 0.024 0.024 0.026
[217] 0.210 0.210 0.210 0.210 0.210 0.210 0.212 0.214 0.216
[226] 0.218 0.220 0.222 0.224 0.224 0.226 0.228 0.228 0.230
[235] 0.230 0.230 0.232 0.232 0.234 0.234 0.236 0.238 0.240
[244] 0.240 0.240 0.242 0.242 0.242 0.242 0.244 0.246 0.248
[478] 0.482 0.484 0.484 0.486 0.486 0.486 0.488 0.490 0.492
[487] 0.492 0.492 0.492 0.494 0.494 0.496 0.498 0.498 0.498
[496] 0.498 0.498 0.500 0.500 0.502

> # https://www.statmethods.net/advgraphs/parameters.html
> # cex.axisはx軸の刻みの数字の大きさを調整
> # cex.mainはtitleの文字の大きさを調整
> plot( n , runProp , type="o" , log="x" , col="skyblue" , xlim=c(1,N) , ylim=c(0.0,1.0) , cex.axis=1.5 ,xlab="Flip Number" , ylab="Proportion Heads" , cex.lab=1.5 ,main="Running Proportion of Heads" , cex.main=1.5 )

> # 0.5の境目の線を引く
> abline( h=pHeads , lty="dotted" )

> # flipLettersは "HTTHHTHTHT"のような文字列
> flipLetters = paste( c("T","H")[flipSequence[1:10]+1] , collapse="" )

> displayString = paste0( "Flip Sequence=", flipLetters , "..." )
> text( N , .9 , displayString , adj=c(1,0.5) , cex=1.3 )
> text( N , .8 , paste("End Proportion =",runProp[N]) , adj=c(1,0.5) , cex=1.3 )
> 
{{< /highlight >}}

![png](../../simulate-coin-flip-using-r/1.png) 
