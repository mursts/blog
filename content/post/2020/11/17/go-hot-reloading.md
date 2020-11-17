---
title:       "GoのWebアプリ開発にReflexを使ってホットリロードする"
subtitle:    ""
description: ""
date:        2020-11-17
author:      ""
image:       ""
tags:        ["go"]
slug:        "go-reflex-hot-reloading"
---

GoのWebアプリ開発でホットリロードをやりたくて [Reflex](https://github.com/cespare/reflex) を使ってみたのでメモ  
Reflexの他に [Realize](https://github.com/oxequa/realize) 使われているみたいだけど、設定ファイルが必要になるのでコマンドだけで完結できるReflexを選択した。

# インストール

```sh
$ go get github.com/cespare/reflex
```

# 使い方

以下のようなファイルがある場合、

```
❯ tree .
.
└── main.go
```

```go
package main

import (
    "fmt"
    "net/http"
    "os"
)

func handler(w http.ResponseWriter, _ *http.Request) {
    _, _ = fmt.Fprintln(w, "Hello World")
}

func main() {
    http.HandleFunc("/", handler)
    port := os.Getenv("port")
    if port == "" {
        port = "8080"
    }
    if err := http.ListenAndServe(":"+port, nil); err != nil {
        os.Exit(1)
    }
}
```

このコマンドを実行するだけ

```sh
reflex -r '\.go$' -s go run main.go
```

複数の拡張子を対象にする場合

```sh
reflex -r '(\.go$|\.js$)' -s go run main.go
```

Golandの `.idea` ディレクトリを無視する場合

```sh
reflex -r '\.go$' -R '^.idea/' -s go run main.go
```

ファイル数が多いサブディレクトリを監視対象にするとエラーになるようなので、JSの `node_modules` は無視しないとダメそう
`.git` はデフォルトで無視される

他にもいろいろ指定できるけど単純なアプリの場合はこれだけで一旦OK  
ReflexはGoに限らずinotify-toolsのように何にでも使えそう
