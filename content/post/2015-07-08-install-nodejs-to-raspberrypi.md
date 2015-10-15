+++
date = "2015-07-08T01:39:33+09:00"
slug = "install-nodejs-to-raspberrypi"
title = "Raspberry PiにNode.jsをインストールする"
tags = ["Raspberry Pi", "Nodejs"]
+++

## 追記
Node.jsのバージョン4でソースからのインストールが不要になったので、[別記事](/entry/2015/10/13/install-nodejs-v4-to-raspberrypi2)でインストール手順を書きました。

---

ndevnを使ってNode.jsのバージョンを管理する環境を作りました

[ndenv](https://github.com/riywo/ndenv)

今回はまっさらなRaspberry Pi 2(Raspbian)にインストール

<!--more-->

```
$ uname -a
Linux raspberrypi 3.18.12-v7+ #783 SMP PREEMPT Tue May 5 22:48:52 BST 2015 armv7l GNU/Linux
```

## ndenvをインストール

```bash
$ git clone https://github.com/riywo/ndenv ~/.ndenv
$ echo 'export PATH="$HOME/.ndenv/bin:$PATH"' >> ~/.profile
$ echo 'eval "$(ndenv init -)"' >> ~/.profile
$ exec $SHELL -l
```

## node-buildをインストール

[node-build](https://github.com/riywo/node-build)をインストールすると`ndenv install`コマンドが使えるようになってNodeのインストールができるようになります。

```
git clone https://github.com/riywo/node-build.git ~/.ndenv/plugins/node-build
```

## インストールできるバージョンの確認

```
$ edenv install -l
```

## Nodeをインストール(失敗)

```
$ ndenv install v0.12.6
pushd: node-v0.12.6-linux-armv7l: No such file or directory
```
最新(2015/07/08時点)のv0.12.6をインストールしようとしたらエラーになりました。
node-buildがarmv7lに対応してなかったり、raspi用のバイナリがないようです。

http://nodejs.org/dist/v0.12.6/

## Nodeをソースからインストール

ndenvで使用できるように`$HOME/.ndenv/versions`にインストール
```
$ mkdir ~/.ndenv/versions/v0.12.6
$ mkdir ~/source
$ cd source
$ wget http://nodejs.org/dist/v0.12.6/node-v0.12.6.tar.gz
$ tar xvf node-v0.12.6.tar.gz
$ cd node-v0.12.6
$ ./configure --prefix=$HOME/.ndenv/versions/v0.12.6/
$ make
$ make install

$ ndenv rehash
$ ndenv global v0.12.6
$ node -v
v0.12.6

$ npm -v #npmもインストールされます
2.11.2
```

**makeに2時間位かかりましたが、`make -j 4`とすると4コアをフル活用して処理時間が短くなるみたいです。**

ソースからコンパイルするのは・・・という場合は、[node-arm](http://node-arm.herokuapp.com)からraspiに対応したバイナリをダウンロードできます
