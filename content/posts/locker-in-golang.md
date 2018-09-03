---
title: "Golangでの並列処理サンプル"
date: 2018-07-19T11:13:00+09:00
draft: false
---


### 例1
1000個のgoroutineで`somekey`を更新するプログラムで、更新する際に`sync.Mutex`でロックをかける。

{{< highlight golang >}}
package main

import (
    "fmt"
    "sync"
    "time"
)

type SafeCounter struct {
    v   map[string]int
    mux sync.Mutex
}

func (c *SafeCounter) Inc(key string) {
    c.mux.Lock()
    c.v[key]++
    c.mux.Unlock()
}

func (c *SafeCounter) Value(key string) int {
    c.mux.Lock()
    defer c.mux.Unlock()
    return c.v[key]
}

func main() {
    c := SafeCounter{v: make(map[string]int)}
    for i := 0; i < 1000; i++ {
        go c.Inc("somekey")
    }
    time.Sleep(time.Second) // 待たないとと、974, 949などの値が出てくる
    fmt.Println(c.Value("somekey"))
}
{{< / highlight >}}

### 例2

例1は`time.Sleep`関数で1000個のgoroutineを待っていたが、`sync.WaitGroup`を使って、1000個のgoroutineが全部終了するのをまつこともできる。
例1の`main`関数の部分だけを修正する。

{{< highlight golang >}}
func main() {
    c := SafeCounter{v: make(map[string]int)}

    var wg sync.WaitGroup
    for i := 0; i < 1020; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            c.Inc("somekey")
        }()
    }
    wg.Wait()
    fmt.Println(c.Value("somekey"))
}
{{< / highlight >}}
