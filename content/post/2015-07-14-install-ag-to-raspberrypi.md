+++
date = "2015-07-14T01:11:49+09:00"
slug = "install-ag-to-raspberrypi"
tags = ["Raspberry Pi", "The Silver Searcher"]
title = "Raspberry PiにThe Silver Searcherをインストール"

+++

Raspberry Pi(Raspbian)にThe Silver Searchar(ag)をインストールしました。

https://github.com/ggreer/the_silver_searcher

<!--more-->

apt-getではインストールできなかったのでソースからインストール。

```
$ sudo apt-get install automake pkg-config libpcre3-dev zlib1g-dev liblzma-dev
$ wget http://geoff.greer.fm/ag/releases/the_silver_searcher-0.30.0.tar.gz
$ tar -zxvf the_silver_searcher-0.30.0.tar.gz
$ cd the_silver_searcher-0.30.0
$ ./configure --prefix=$HOME/usr #今回はユーザディレクトリにインストール
$ make
$ make install

$ ag --version
ag version 0.30.0

Features:
  +jit +lzma +zlib
```
