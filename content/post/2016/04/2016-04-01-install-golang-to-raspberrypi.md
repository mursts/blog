+++
date = "2016-04-01T23:15:48+09:00"
slug = "install-golang-to-raspberrypi"
tags = ["Go", "Raspberry Pi"]
title = "RaspberryPiにGoをインストールする"

+++

RaspberryPiにGoをインストールするのにいつものapt-getでインストールすると

```sh
$ apt-get install golang

$ go version
go version go1.3.3 linux/arm
```

<!--more-->

なんと`1.3`でした。2016/04/01の現在の最新は`1.6`です。

今更1.3もないので、、[Getting Started](https://golang.org/doc/install)を見ながらインストールしました。

ダウンロード後に`/usr/local`にファイル展開して、`/usr/local/go/bin`へパスを通して完了です

```sh
$ wget https://storage.googleapis.com/golang/go1.6.linux-armv6l.tar.gz
$ sudo tar -C /usr/local -xzf go1.6.linux-armv6l.tar.gz

$ go version
go version go1.6 linux/arm
```

これだけでインストールできているようです。
