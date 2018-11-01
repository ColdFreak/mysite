---
title: "R入門2"
date: "2018-10-23T12:10:30+09:00"
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

Rの入門2,　簡単にdata frameを操作する

### mtcarsデータセット

特にロードする必要もなく、デフォルトで入っている。


{{< highlight r >}}
mtcars
{{< / highlight >}}

                         mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
    Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
    Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
    Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
    Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
    Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
    Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
    Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
    Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
    Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
    Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
    Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
    AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
    Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
    Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
    Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
    Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2


`mtcars` data frameの各種サマリーを表示


{{< highlight r >}}
print(str(mtcars))
{{< / highlight >}}

    'data.frame':	32 obs. of  11 variables:
     $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
     $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
     $ disp: num  160 160 108 258 360 ...
     $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
     $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
     $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
     $ qsec: num  16.5 17 18.6 19.4 17 ...
     $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
     $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
     $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
     $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
    NULL


carbカラムのデータタイプを調べる


{{< highlight r >}}
print(typeof(mtcars$carb))
{{< / highlight >}}

    [1] "double"


carbカラムにユニークな値


{{< highlight r >}}
print(unique(mtcars$carb))
{{< / highlight >}}

    [1] 4 1 2 3 6 8


carbカラムに1は７個、2は10個などの情報を出す


{{< highlight r >}}
print(table(mtcars$carb))
{{< / highlight >}}

    
     1  2  3  4  6  8 
     7 10  3 10  1  1 

`quantile()`関数を使って、mpg列の四分位点を調べる


{{< highlight r >}}
print(quantile(mtcars$mpg))
{{< / highlight >}}

        0%    25%    50%    75%   100% 
    10.400 15.425 19.200 22.800 33.900 



### airqualityデータセット


{{< highlight r >}}
airquality
{{< / highlight >}}


<table>
<thead><tr><th scope=col>Ozone</th><th scope=col>Solar.R</th><th scope=col>Wind</th><th scope=col>Temp</th><th scope=col>Month</th><th scope=col>Day</th></tr></thead>
<tbody>
	<tr><td> 41 </td><td>190 </td><td> 7.4</td><td>67  </td><td>5   </td><td> 1  </td></tr>
	<tr><td> 36 </td><td>118 </td><td> 8.0</td><td>72  </td><td>5   </td><td> 2  </td></tr>
	<tr><td> 12 </td><td>149 </td><td>12.6</td><td>74  </td><td>5   </td><td> 3  </td></tr>
	<tr><td> 18 </td><td>313 </td><td>11.5</td><td>62  </td><td>5   </td><td> 4  </td></tr>
	<tr><td> NA </td><td> NA </td><td>14.3</td><td>56  </td><td>5   </td><td> 5  </td></tr>
	<tr><td> 28 </td><td> NA </td><td>14.9</td><td>66  </td><td>5   </td><td> 6  </td></tr>
	<tr><td> 23 </td><td>299 </td><td> 8.6</td><td>65  </td><td>5   </td><td> 7  </td></tr>
	<tr><td> 19 </td><td> 99 </td><td>13.8</td><td>59  </td><td>5   </td><td> 8  </td></tr>
	<tr><td>  8 </td><td> 19 </td><td>20.1</td><td>61  </td><td>5   </td><td> 9  </td></tr>
	<tr><td> NA </td><td>194 </td><td> 8.6</td><td>69  </td><td>5   </td><td>10  </td></tr>
	<tr><td>  7 </td><td> NA </td><td> 6.9</td><td>74  </td><td>5   </td><td>11  </td></tr>
	<tr><td> 16 </td><td>256 </td><td> 9.7</td><td>69  </td><td>5   </td><td>12  </td></tr>
	<tr><td> 11 </td><td>290 </td><td> 9.2</td><td>66  </td><td>5   </td><td>13  </td></tr>
	<tr><td> 14 </td><td>274 </td><td>10.9</td><td>68  </td><td>5   </td><td>14  </td></tr>
	<tr><td> 18 </td><td> 65 </td><td>13.2</td><td>58  </td><td>5   </td><td>15  </td></tr>
	<tr><td> 14 </td><td>334 </td><td>11.5</td><td>64  </td><td>5   </td><td>16  </td></tr>
	<tr><td> 34 </td><td>307 </td><td>12.0</td><td>66  </td><td>5   </td><td>17  </td></tr>
	<tr><td>  6 </td><td> 78 </td><td>18.4</td><td>57  </td><td>5   </td><td>18  </td></tr>
	<tr><td> 30 </td><td>322 </td><td>11.5</td><td>68  </td><td>5   </td><td>19  </td></tr>
	<tr><td> 11 </td><td> 44 </td><td> 9.7</td><td>62  </td><td>5   </td><td>20  </td></tr>
	<tr><td>  1 </td><td>  8 </td><td> 9.7</td><td>59  </td><td>5   </td><td>21  </td></tr>
	<tr><td> 11 </td><td>320 </td><td>16.6</td><td>73  </td><td>5   </td><td>22  </td></tr>
	<tr><td>  4 </td><td> 25 </td><td> 9.7</td><td>61  </td><td>5   </td><td>23  </td></tr>
	<tr><td> 32 </td><td> 92 </td><td>12.0</td><td>61  </td><td>5   </td><td>24  </td></tr>
	<tr><td> NA </td><td> 66 </td><td>16.6</td><td>57  </td><td>5   </td><td>25  </td></tr>
	<tr><td> NA </td><td>266 </td><td>14.9</td><td>58  </td><td>5   </td><td>26  </td></tr>
	<tr><td> NA </td><td> NA </td><td> 8.0</td><td>57  </td><td>5   </td><td>27  </td></tr>
	<tr><td> 23 </td><td> 13 </td><td>12.0</td><td>67  </td><td>5   </td><td>28  </td></tr>
	<tr><td> 45 </td><td>252 </td><td>14.9</td><td>81  </td><td>5   </td><td>29  </td></tr>
	<tr><td>115 </td><td>223 </td><td> 5.7</td><td>79  </td><td>5   </td><td>30  </td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>96  </td><td>167 </td><td> 6.9</td><td>91  </td><td>9   </td><td> 1  </td></tr>
	<tr><td>78  </td><td>197 </td><td> 5.1</td><td>92  </td><td>9   </td><td> 2  </td></tr>
	<tr><td>73  </td><td>183 </td><td> 2.8</td><td>93  </td><td>9   </td><td> 3  </td></tr>
	<tr><td>91  </td><td>189 </td><td> 4.6</td><td>93  </td><td>9   </td><td> 4  </td></tr>
	<tr><td>47  </td><td> 95 </td><td> 7.4</td><td>87  </td><td>9   </td><td> 5  </td></tr>
	<tr><td>32  </td><td> 92 </td><td>15.5</td><td>84  </td><td>9   </td><td> 6  </td></tr>
	<tr><td>20  </td><td>252 </td><td>10.9</td><td>80  </td><td>9   </td><td> 7  </td></tr>
	<tr><td>23  </td><td>220 </td><td>10.3</td><td>78  </td><td>9   </td><td> 8  </td></tr>
	<tr><td>21  </td><td>230 </td><td>10.9</td><td>75  </td><td>9   </td><td> 9  </td></tr>
	<tr><td>24  </td><td>259 </td><td> 9.7</td><td>73  </td><td>9   </td><td>10  </td></tr>
	<tr><td>44  </td><td>236 </td><td>14.9</td><td>81  </td><td>9   </td><td>11  </td></tr>
	<tr><td>21  </td><td>259 </td><td>15.5</td><td>76  </td><td>9   </td><td>12  </td></tr>
	<tr><td>28  </td><td>238 </td><td> 6.3</td><td>77  </td><td>9   </td><td>13  </td></tr>
	<tr><td> 9  </td><td> 24 </td><td>10.9</td><td>71  </td><td>9   </td><td>14  </td></tr>
	<tr><td>13  </td><td>112 </td><td>11.5</td><td>71  </td><td>9   </td><td>15  </td></tr>
	<tr><td>46  </td><td>237 </td><td> 6.9</td><td>78  </td><td>9   </td><td>16  </td></tr>
	<tr><td>18  </td><td>224 </td><td>13.8</td><td>67  </td><td>9   </td><td>17  </td></tr>
	<tr><td>13  </td><td> 27 </td><td>10.3</td><td>76  </td><td>9   </td><td>18  </td></tr>
	<tr><td>24  </td><td>238 </td><td>10.3</td><td>68  </td><td>9   </td><td>19  </td></tr>
	<tr><td>16  </td><td>201 </td><td> 8.0</td><td>82  </td><td>9   </td><td>20  </td></tr>
	<tr><td>13  </td><td>238 </td><td>12.6</td><td>64  </td><td>9   </td><td>21  </td></tr>
	<tr><td>23  </td><td> 14 </td><td> 9.2</td><td>71  </td><td>9   </td><td>22  </td></tr>
	<tr><td>36  </td><td>139 </td><td>10.3</td><td>81  </td><td>9   </td><td>23  </td></tr>
	<tr><td> 7  </td><td> 49 </td><td>10.3</td><td>69  </td><td>9   </td><td>24  </td></tr>
	<tr><td>14  </td><td> 20 </td><td>16.6</td><td>63  </td><td>9   </td><td>25  </td></tr>
	<tr><td>30  </td><td>193 </td><td> 6.9</td><td>70  </td><td>9   </td><td>26  </td></tr>
	<tr><td>NA  </td><td>145 </td><td>13.2</td><td>77  </td><td>9   </td><td>27  </td></tr>
	<tr><td>14  </td><td>191 </td><td>14.3</td><td>75  </td><td>9   </td><td>28  </td></tr>
	<tr><td>18  </td><td>131 </td><td> 8.0</td><td>76  </td><td>9   </td><td>29  </td></tr>
	<tr><td>20  </td><td>223 </td><td>11.5</td><td>68  </td><td>9   </td><td>30  </td></tr>
</tbody>
</table>




{{< highlight r >}}
print(str(airquality))
{{< / highlight >}}

    'data.frame':	153 obs. of  6 variables:
     $ Ozone  : int  41 36 12 18 NA 28 23 19 8 NA ...
     $ Solar.R: int  190 118 149 313 NA NA 299 99 19 194 ...
     $ Wind   : num  7.4 8 12.6 11.5 14.3 14.9 8.6 13.8 20.1 8.6 ...
     $ Temp   : int  67 72 74 62 56 66 65 59 61 69 ...
     $ Month  : int  5 5 5 5 5 5 5 5 5 5 ...
     $ Day    : int  1 2 3 4 5 6 7 8 9 10 ...
    NULL


construct `9` number of equally-spaced bins for us by using the cut function. 間隔は同じで、その間隔に入っている数字の数は違うかも。table関数でみることができる


{{< highlight r >}}
print(cut(airquality$Temp, 9))
{{< / highlight >}}

      [1] (65.1,69.7] (69.7,74.2] (69.7,74.2] (60.6,65.1] (56,60.6]   (65.1,69.7]
      [7] (60.6,65.1] (56,60.6]   (60.6,65.1] (65.1,69.7] (69.7,74.2] (65.1,69.7]
     [13] (65.1,69.7] (65.1,69.7] (56,60.6]   (60.6,65.1] (65.1,69.7] (56,60.6]  
     [19] (65.1,69.7] (60.6,65.1] (56,60.6]   (69.7,74.2] (60.6,65.1] (60.6,65.1]
     [25] (56,60.6]   (56,60.6]   (56,60.6]   (65.1,69.7] (78.8,83.3] (78.8,83.3]
     [31] (74.2,78.8] (74.2,78.8] (69.7,74.2] (65.1,69.7] (83.3,87.9] (83.3,87.9]
     [37] (78.8,83.3] (78.8,83.3] (83.3,87.9] (87.9,92.4] (83.3,87.9] (92.4,97]  
     [43] (87.9,92.4] (78.8,83.3] (78.8,83.3] (78.8,83.3] (74.2,78.8] (69.7,74.2]
     [49] (60.6,65.1] (69.7,74.2] (74.2,78.8] (74.2,78.8] (74.2,78.8] (74.2,78.8]
     [55] (74.2,78.8] (74.2,78.8] (74.2,78.8] (69.7,74.2] (78.8,83.3] (74.2,78.8]
     [61] (78.8,83.3] (83.3,87.9] (83.3,87.9] (78.8,83.3] (83.3,87.9] (78.8,83.3]
     [67] (78.8,83.3] (87.9,92.4] (87.9,92.4] (87.9,92.4] (87.9,92.4] (78.8,83.3]
     [73] (69.7,74.2] (78.8,83.3] (87.9,92.4] (78.8,83.3] (78.8,83.3] (78.8,83.3]
     [79] (83.3,87.9] (83.3,87.9] (83.3,87.9] (69.7,74.2] (78.8,83.3] (78.8,83.3]
     [85] (83.3,87.9] (83.3,87.9] (78.8,83.3] (83.3,87.9] (87.9,92.4] (83.3,87.9]
     [91] (78.8,83.3] (78.8,83.3] (78.8,83.3] (78.8,83.3] (78.8,83.3] (83.3,87.9]
     [97] (83.3,87.9] (83.3,87.9] (87.9,92.4] (87.9,92.4] (87.9,92.4] (87.9,92.4]
    [103] (83.3,87.9] (83.3,87.9] (78.8,83.3] (78.8,83.3] (78.8,83.3] (74.2,78.8]
    [109] (78.8,83.3] (74.2,78.8] (74.2,78.8] (74.2,78.8] (74.2,78.8] (69.7,74.2]
    [115] (74.2,78.8] (78.8,83.3] (78.8,83.3] (83.3,87.9] (87.9,92.4] (92.4,97]  
    [121] (92.4,97]   (92.4,97]   (92.4,97]   (87.9,92.4] (87.9,92.4] (92.4,97]  
    [127] (92.4,97]   (83.3,87.9] (83.3,87.9] (78.8,83.3] (74.2,78.8] (74.2,78.8]
    [133] (69.7,74.2] (78.8,83.3] (74.2,78.8] (74.2,78.8] (69.7,74.2] (69.7,74.2]
    [139] (74.2,78.8] (65.1,69.7] (74.2,78.8] (65.1,69.7] (78.8,83.3] (60.6,65.1]
    [145] (69.7,74.2] (78.8,83.3] (65.1,69.7] (60.6,65.1] (69.7,74.2] (74.2,78.8]
    [151] (74.2,78.8] (74.2,78.8] (65.1,69.7]
    9 Levels: (56,60.6] (60.6,65.1] (65.1,69.7] (69.7,74.2] ... (92.4,97]



{{< highlight r >}}
# それぞれの区間内にどれだけ入っているかを調べる
print(table(cut(airquality$Temp, 9)))
{{< / highlight >}}

    
      (56,60.6] (60.6,65.1] (65.1,69.7] (69.7,74.2] (74.2,78.8] (78.8,83.3] 
              8          10          14          16          26          35 
    (83.3,87.9] (87.9,92.4]   (92.4,97] 
             22          15           7 


carbは１である確率は0.2185, これはPMF(probability mass function)と呼ばれる


{{< highlight r >}}
print(table(mtcars$carb))
print(table(mtcars$carb) / length(mtcars$carb))
{{< / highlight >}}

    
     1  2  3  4  6  8 
     7 10  3 10  1  1 
    
          1       2       3       4       6       8 
    0.21875 0.31250 0.09375 0.31250 0.03125 0.03125 

