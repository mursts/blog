---
title:       "ChromeOSのCrostiniで日本語入力する"
subtitle:    ""
description: ""
date:        2020-08-06
author:      ""
image:       ""
tags:        ["chromeos", "pixelbookgo"]
slug:        "setting-mozc-on-chromebook"
---

Crostiniではデフォルトでは日本語入力できないのでMozcを導入する  
> [Crostiniで日本語環境設定:健康で文化的な最低限度の生活のためのChromebook設定(2)](https://scrapbox.io/hada/Crostini%E3%81%A7%E6%97%A5%E6%9C%AC%E8%AA%9E%E7%92%B0%E5%A2%83%E8%A8%AD%E5%AE%9A:%E5%81%A5%E5%BA%B7%E3%81%A7%E6%96%87%E5%8C%96%E7%9A%84%E3%81%AA%E6%9C%80%E4%BD%8E%E9%99%90%E5%BA%A6%E3%81%AE%E7%94%9F%E6%B4%BB%E3%81%AE%E3%81%9F%E3%82%81%E3%81%AEChromebook%E8%A8%AD%E5%AE%9A(2))

やることは参考サイトのまま

## Mozcをインストールする  
aptでインストールするだけ
```
$ sudo apt install fcitx-mozc
```

fcitxはインプットメソッドフレームワークのことらしい。そのフレームワーク上でMozcを動かすイメージかな？
> https://ja.wikipedia.org/wiki/Fcitx

インストールが終わったらfxitxを起動
```sh
$ export XMODIFIERS=@im=fcitx
#fxitxを起動するだけで自動起動の設定は後ほど
$ fcitx-autostart
```
警告とか出るけどきにしない

MozcをFcitxの入力メソッドに追加する
```sh
# 設定を開く
$ fcitx-configtool
```
右下の+ボタンを押してMozcを追加する
![](/post/2020/08/06/mozc02.jpg)
追加後
![](/post/2020/08/06/mozc01.png)

これでctrl+spaceで日本語入力できるようになる

## Mozcの設定
このコマンドでMozcの設定を変更するダイアログが出てくる
```sh
$ /usr/lib/mozc/mozc_tool --mode=config_dialog
```

## 自動起動の設定

下３行を`/etc/systemd/user/cros-garcon.service.d/cros-garcon-override.conf` に環境変数を追加
```txt
Environment="GTK_IM_MODULE=fcitx"
Environment="QT_IM_MODULE=fcitx"
Environment="XMODIFIERS=@im=fcitx"
```
`fxitx-autostart`を実行する設定を入れる
```sh
echo "/usr/bin/fcitx-autostart" >> ~/.sommelierrc
```

最後にログインして自動起動が有効になっているか確認して終了
