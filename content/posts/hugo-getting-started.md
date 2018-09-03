---
title: "Hugo Getting Started"
date: 2018-07-01T02:02:36+09:00
draft: false
---

ローカルのMacでhugoを動かす。

```
$ brew install hugo

$ hugo version
Hugo Static Site Generator v0.42.2 darwin/amd64

$ hugo new site mysite

$ cd mysite/

$ git init

$ git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke

$ echo 'theme = "ananke"' >> config.toml

$ hugo new posts/install-golang.md
/Users/wang/work/github/mysite/content/posts/install-golang.md created

$ hugo server -D
```

http://localhost:1313/ へアクセスする。
