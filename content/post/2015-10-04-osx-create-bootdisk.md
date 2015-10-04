+++
date = "2015-10-04T21:41:50+09:00"
slug = "osx-create-bootdisk"
tags = ["OSX"]
title = "Mac OS Xの起動ディスクを作成してクリーンインストールする手順メモ"

+++

Mac OS X 10.11(El Capitan)をUSBメモリに起動ディスクを作成して、クリーンインストールした時の手順

<!--more-->

## App StoreからOSをダウンロード

App Storeを起動してアップグレードを選択する。

ダウンロードが終わったら、アップグレードを続けずにcommand + q などで終了する

## USBメモリを初期化する

ディスクユーティリティから初期化

初期化の際に名称を`名称未設定`から英語にしておくといいかも。今回は`UNTITLED`に設定した

## 起動ディスクを作成

```sh
$ sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/UNTITLED/ --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app --nointeraction
```

`--volume /Volumes/UNTITLED`は初期化した時の名前を設定

## インストールする

オプションボタンを押しながら再起動する。

後は画面の流れでインストール。
