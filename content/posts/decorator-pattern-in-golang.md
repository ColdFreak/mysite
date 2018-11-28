---
title: "Decorator Pattern in Golang"
date: "2018-11-28T15:32:51+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "golang"
  - "decorator"
tags:
  - "golang"
---

Decorator パターン既存のオブジェクトに新しい機能や振る舞いを動的に追加することを可能にする。
関数Aに一つの関数Bのタイプを引数として渡して、関数Aに機能追加した後に、同じタイプの関数を返す仕組み。

したのコードは文字列を処理するために、一個の機能を追加する（文字列を小文字にする）。


{{< highlight go >}}
package main

import (
        // "encoding/base64"
        "fmt"
        "strings"
)

type StringManipulator func(string) string

func ToLower(m StringManipulator) StringManipulator {

        return func(s string) string {
                lower := strings.ToLower(s)
                return m(lower)
        }
}

func ident(s string) string {
        return s
}
func main() {

        s := "Hello Playground"

        var fn StringManipulator = ident
        fmt.Println(fn(s))

        fn = ToLower(fn)
        fmt.Println(fn(s))
}
{{< /highlight >}}


https://stackoverflow.com/questions/45944781/decorator-functions-in-go
