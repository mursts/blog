+++
date = "2021-05-04"
slug = "wine-kindle-for-pc"
title = "Linux環境にKindle for PCをインストールする"
tags = ["linux", "kindle", "wine"]
isCJKLanguage = true
+++

Linux用にKindleのViewerが用意されてないのでWineを使ってKindle for PCのexeを動かす


## 環境

```sh
$ uname -a
Linux HomeMachine 5.10.32-1-MANJARO #1 SMP Wed Apr 21 14:54:10 UTC 2021 x86_64 GNU/Linux
```

## インストール

pacmanで必要なものをインストールする

```sh
$ sudo pacman -S wine winetricks lib32-gnutls
```

## 必要なディレクトリを作成する

{{< tweet user="mursts" id="1360561626718474240" >}}

これを作らないと最新版のKindleがエラーで動かなかった。古いバージョンではなくても大丈夫。

## winetricksでフォントをインストールする

いわゆる豆腐対策

```sh
$ winetricks
```

[Select the default wineprefix] - [Install a font]

## 実行

```sh
wine Kindle_for_PC_Windows.exe
```

しかし、実行してもネットワークエラーでインターネットにつながらない


## Wineをダウングレード

ネットワークエラーはWineをダウングレードすることで解消した。  
pacmanでインストールしたのは6.7だったのでこの時の安定版の6.0をインストールした

ここから必要なファイルをダウンロード

https://archive.org/download/archlinux_pkg_wine  

- wine-6.0-1-x86_64.pkg.tar.zst
- wine-6.0-1-x86_64.pkg.tar.zst.sig

```sh
$ sudo pacman -U wine-6.0-1-x86_64.pkg.tar.zst
```


## 再度実行

```sh
wine Kindle_for_PC_Windows.exe
```

これで問題なくKindleが動くようになった


