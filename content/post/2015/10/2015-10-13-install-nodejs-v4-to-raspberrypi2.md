+++
date = "2015-10-13T00:10:02+09:00"
slug = "install-nodejs-v4-to-raspberrypi2"
tags = ["Raspberry Pi", "Nodejs"]
title = "Raspberry Pi2にNode.jsのv4をインストールする"

+++

[この記事](http://blog.mursts.jp/entry/2015/07/08/install-nodejs-to-raspberrypi/)でRaspiにNode.jsをソースからインストールしてましたが、バージョン4で再度ARMをサポートしてくれたのでファイルを展開するだけでNode.jsが使えるようになりました。

インストール方法は以下のとおり。

<!--more-->

* 2015/10/13時点の最新版をインストール
* インストール先は`$HOME/usr`

```sh
$ wget http://nodejs.org/dist/v4.1.2/node-v4.1.2-linux-armv7l.tar.gz
$ tar -zxf node-v4.1.2-linux-armv7l.tar.gz
$ cd node-v4.1.2-linux-armv7l
$ cp -R ./* ~/usr/
$ rm -rf ~/usr/CHANGELOG.md ~/usr/LICENSE ~/usr/README.md # 不要なファイルを削除
```

忘れずに`$HOME/usr/bin`にパスを通すこと

```sh
$ node -v
v4.1.2

$ npm -v
2.14.4
```

これだけでインストール完了
