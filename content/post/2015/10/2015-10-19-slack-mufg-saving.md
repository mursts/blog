+++
date = "2015-10-19T00:20:51+09:00"
slug = "slack-bot-mufg-banking"
tags = ["Slack", "Python", "Selenium", "Raspberry Pi"]
title = "MUFGの残高照会botを作った話"

+++

みずほ銀行がこんなサービスを始めたそうですね。

> [LINEでかんたん残高照会](http://www.mizuhobank.co.jp/net_shoukai/line/index.html)

LINEでスタンプを送ると残高や直近の入出金明細を教えてくれるそうです。

このネタに乗っかるべくMUFG版をSlackで作ってみました。

※LINEでも非公式APIがあるのでできなくはないが、怖いのでSlackにしました。

<!--more-->

## 動作イメージ

mufgチャンネルで`:moneybag:`を送るとbotが残高を返してくれます。

![](/post/2015/10/slack-mufg.jpg)

## 構成

自宅にあるRaspiからSelenium(+PhantomJS)
でMUFGのサイトをスクレイピングしてSlackに通知しています。自宅の非公開なRaspiなので安全なはず。

あとは、以下の記事のことをしてbotの完成です。(たまたま最近Seleniumでのスクレピングをやっていたので今回のネタにつながりました。)

- [PythonとSeleniumでネットバンキングをスクレイピングする](http://blog.mursts.jp/entry/2015/09/25/scrape-netbanking-by-python-and-selenium/)
- [Raspberry Pi2でPhantomJSをビルドする](http://blog.mursts.jp/entry/2015/10/16/build-phantomjs-on-raspberrypi2/)
- [PythonでSlack botを作ってみる](http://blog.mursts.jp/entry/2015/10/17/create-slack-bot-by-python/)

## まとめ

各銀行各位、LINEと提携してこんなサービスを提供するのではなくAPIを提供してくれるのを願っております

MUFGに関しては夏にAndroidアプリをクソみたいな仕様にしてくれたので、携帯からはSlackを使って口座振り込み等もChatBanking的な感じでやってやろうかと計画中です。
