+++
date = "2015-08-02T14:06:34+09:00"
slug = "install-jq-in-raspberrypi"
tags = ["Raspberry Pi", "jq"]
title = "Raspberry Piにjqをインストール"

+++

aptではjqをインストールができなさそうだったのでソースからインストールしました。

<!--more-->

```
$ wget http://stedolan.github.io/jq/download/source/jq-1.4.tar.gz
$ tar -zxvf jq-1.4.tar.gz
$ cd jq-1.4/
$ ./configure
$ make -j 4
$ sudo make install
$ jq --version
jq-1.4
```
