---
title: "R入門3"
date: "2018-10-23T22:40:09+09:00"
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

`ggplot2()`の基礎

{{< highlight r >}}
install.packages("tidyverse")
install.packages("dplyr")
{{< / highlight >}}

    Updating HTML index of packages in '.Library'
    Making 'packages.html' ... done
    Updating HTML index of packages in '.Library'
    Making 'packages.html' ... done


ggplot2 is one of the core members of the `tidyverse` package.


{{< highlight r >}}
library(tidyverse)
{{< / highlight >}}

    ── Attaching packages ─────────────────────────────────────── tidyverse 1.2.1 ──
    ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ✔ tibble  1.4.2     ✔ dplyr   0.7.7
    ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ✔ readr   1.1.1     ✔ forcats 0.3.0
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()


mpgデータセットの説明

`hwy` : highway miles per gallon

`displ` : engine displacement, in litres


{{< highlight r >}}
?mpg
{{< / highlight >}}


{{< highlight r >}}
head(mpg)
{{< / highlight >}}


<table>
<thead><tr><th scope=col>manufacturer</th><th scope=col>model</th><th scope=col>displ</th><th scope=col>year</th><th scope=col>cyl</th><th scope=col>trans</th><th scope=col>drv</th><th scope=col>cty</th><th scope=col>hwy</th><th scope=col>fl</th><th scope=col>class</th></tr></thead>
<tbody>
	<tr><td>audi      </td><td>a4        </td><td>1.8       </td><td>1999      </td><td>4         </td><td>auto(l5)  </td><td>f         </td><td>18        </td><td>29        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>1.8       </td><td>1999      </td><td>4         </td><td>manual(m5)</td><td>f         </td><td>21        </td><td>29        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>2.0       </td><td>2008      </td><td>4         </td><td>manual(m6)</td><td>f         </td><td>20        </td><td>31        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>2.0       </td><td>2008      </td><td>4         </td><td>auto(av)  </td><td>f         </td><td>21        </td><td>30        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>2.8       </td><td>1999      </td><td>6         </td><td>auto(l5)  </td><td>f         </td><td>16        </td><td>26        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>2.8       </td><td>1999      </td><td>6         </td><td>manual(m5)</td><td>f         </td><td>18        </td><td>26        </td><td>p         </td><td>compact   </td></tr>
</tbody>
</table>




{{< highlight r >}}
print(unique(mpg$class))
{{< / highlight >}}

    [1] "compact"    "midsize"    "suv"        "2seater"    "minivan"
    [6] "pickup"     "subcompact"


X軸はdisplで、Y軸はhwy,　エンジンが大きいければ、燃費が悪いことがわかる。
The function `geom_point()` adds a layer of points to your plot, which creates a scatterplot.

The mapping argument is always paired with `aes()`, and the x and y arguments of aes() specify which variables to map to the x- and y-axes.

ggplot2のテンプレートは下のような感じ：

```
ggplot(data = <DATA>) +
      <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))
```


{{< highlight r >}}
ggplot(data=mpg) +
    geom_point(mapping = aes(x=displ, y=hwy))
{{< / highlight >}}


![png](../../r_tutorial_3/output_8_1.png)

クラスごとに色をつけて、わかりやすくする。 `color = class`を`size = class`、`alpha = class`、`shape = class`などにしてもよい

{{< highlight r >}}
ggplot(data=mpg) +
    geom_point(mapping = aes(x = displ, y = hwy, color = class))
{{< / highlight >}}


![png](../../r_tutorial_3/output_10_1.png)

７種類のclassをそれぞれ`displ`と`hwy`のscatterplotを描画するため、`facet_wrap()`を使う

{{< highlight r >}}
ggplot(data = mpg) +
    geom_point(mapping = aes(x = displ, y = hwy)) +
    facet_wrap(~ class, nrow=2)
{{< / highlight >}}

![png](../../r_tutorial_3/output_12_1.png)

A geom is the geometrical object that a plot uses to represent data. People often describe plots by the type of geom that the plot uses. To change the geom in your plot, change the geom function that you add to `ggplot()`


{{< highlight r >}}
ggplot(data=mpg) +
  geom_smooth(mapping = aes(x= displ, y=hwy))
{{< / highlight >}}

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'


![png](../../r_tutorial_3/output_14_2.png)


一枚のプロットにdrvごとに描画する。


{{< highlight r >}}
print(unique(mpg$drv))

ggplot(data = mpg) +
  geom_smooth(mapping = aes(x=displ, y=hwy, linetype=drv))
{{< / highlight >}}

    [1] "f" "4" "r"


    `geom_smooth()` using method = 'loess' and formula 'y ~ x'


![png](../../r_tutorial_3/output_16_3.png)


To display multiple geoms in the same plot, add multiple geom functions to ggplot()


{{< highlight r >}}
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(x = displ, y = hwy))
{{< / highlight >}}

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'



![png](../../r_tutorial_3/output_18_2.png)


ggplot2 includes eight themes by default. Many more are included in add-on packages like ggthemes, by Jef‐ frey Arnold.

They are `theme_bw()`, `theme_light()`, `theme_classic()`, `theme_linedraw()`, `theme_dark()`, `theme_minimal()`, `theme_gray()`and `theme_void()`

{{< highlight r >}}
library(ggplot2)
ggplot(mpg, aes(displ, hwy)) + 
    geom_point(aes(color = class)) + 
    geom_smooth(se = FALSE) + 
    theme_dark()
{{< / highlight >}}
