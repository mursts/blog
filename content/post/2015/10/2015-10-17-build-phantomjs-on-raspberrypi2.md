+++
date = "2015-10-16T23:37:07+09:00"
slug = "build-phantomjs-on-raspberrypi2"
tags = ["PhantomJS", "Raspberry Pi"]
title = "Raspberry Pi2でPhantomJSをビルドする"

+++

先日PhantomJSを使うとSeleniumを使った時にブラウザが起動しないと聞いたので、以下を参考にやってみました。

[How To Compile PhantomJS on the Raspberry Pi 2](http://raspberrypimaker.com/how-to-compile-phantomjs-on-the-raspberry-pi-2/)

※ OSXではHomebrewでインストールできるけど、Raspiの場合はソースからビルドが必要なようです。

<!--more-->

## 必要なパッケージをインストール

```sh
$ sudo apt-get install build-essential g++ flex bison gperf ruby perl libsqlite3-dev libfontconfig1-dev libicu-dev libfreetype6 libssl-dev libpng-dev libjpeg-dev
```

## ソースをもってくる

```sh
$ git clone git://github.com/ariya/phantomjs.git
$ cd phantomjs

# 最新バージョンにチェックアウト
$ git checkout 2.0
```

## ビルド

```sh
$ ./build.sh
```

参考サイトでは `--job 2`で2コアでビルドしていたけど、今回はパラメータ無しでやりました。(`build.sh`を見るとパラメータなしの時は搭載のコア全て使ってくれそうだったので)

結果4時間位かかりました。。。他サイトをみると1時間位でビルドできていたのに。。。

別で6コアのConoHaのVPSでビルドしたら30分かかりませんでした。

ビルドが終わると`bin`ディレクトリに`phantomjs`があります。

```sh
$ bin/phantomjp --version
2.0.1-development
```

## 日本語フォントをインストール

インストールしてないままだと日本語が表示されなかったので。

```sh
$ sudo apt-get install fonts-ipafont
```

## スクリーンショットをとってみる

```sh
$ bin/phantomjs examples/rasterize.js http://phantomjs.org/ phantomjs.png
```

これでブラウザを起動することなくSeleniumでスクレイピングができそうです。
