---

title: "New RelicをEC2にインストール"
date: 2013-12-23
comments: true
tags: ["NewRelic", ""]
---

先日
[JAWS-UG名古屋勉強会](http://jawsug-nagoya.doorkeeper.jp/events/6734)に行ってきました。

LTであった [New Relic](http://newrelic.com)に興味をもったのでEC2にインストールして、サーバのパフォーマンス監視をしてみました。

<!--more-->

## 環境

EC2(micro) Amazon Linux AMI

サーバではRedmineが動いているのみ

## インストール

インストールはドキュメントにあるのをそのまま実行。

プラットフォームは Red Hat or CentOS を選択しました。

```bash
$ sudo rpm -Uvh http://download.newrelic.com/pub/newrelic/el5/i386/newrelic-repo-5-3.noarch.rpm
$ sudo yum install newrelic-sysmond
$ sudo nrsysmond-config --set license_key="ライセンスキー"
$ sudo /etc/init.d/newrelic-sysmond start
```

インストールして数分後に以下のように表示されます。

![](/post/server_monitoring.jpg)

サーバの監視の他に、スマートフォンアプリやWebアプリの監視もできるようです。

## おまけ

今回サーバの他に自作のWebアプリの監視をやってみたところ、以下のようなメールがきました。

![](/post/NewRelicShirt.jpg)

どうやらTシャツをプレゼントしてくれるようです。

SNSにアップするのを忘れないようにということなので、忘れないようにします。
