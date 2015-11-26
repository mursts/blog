+++
date = "2015-11-26T23:36:21+09:00"
slug = "install-inkscape-to-osx"
tags = ["Inkscape"]
title = "InkscapeをMacにインストールする"

+++

Inkscapeだけをインストールしてもうまく起動しなかったので覚書

## 環境

- Max OS X Yosemite
- Inkscape 0.91

<!--more-->

## Inkscapeをインストール

ここからダウンロード

[Download | Inkscape](https://inkscape.org/ja/download/)

dmgファイルをダウンロードして実行する

## XQuartzをインストール

ダウンロードサイトに以下のようにあり`XQuartz`が必須のようです。

> Download this .dmg file (requires XQuartz)

これもダウンロードして実行する

[XQuartz](http://www.xquartz.org/)

## 起動シェルを修正

自分の環境ではまた起動しなかったので以下のブログを参考にシェルを修正しました。

[InkscapeのMac（OSX Yosemite 10.10.5）へのインストール](http://mmorley.hatenablog.com/entry/2015/08/28/162802)

`/Applications/Inkscape.app/Contents/Resources/bin/inkscape`の以下の3箇所を修正

- export LANG="en_US.UTF-8"　→　export LANG="ja_JP.UTF-8"
- export LANG="en_US.UTF-8"　→　export LANG="ja_JP.UTF-8"
- export LANG="$tmpLANG.UTF-8"　→　export LANG="ja_JP.UTF-8"

これを修正して起動することができました。
