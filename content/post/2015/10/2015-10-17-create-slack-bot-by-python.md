+++
date = "2015-10-17T14:56:54+09:00"
slug = "create-slack-bot-by-python"
tags = ["Python", "Slack"]
title = "PythonでSlack botを作ってみる"

+++

自宅のRaspberry PiでHubotでSlackbotを作って動かしてますが、Hubotの場合CoffeeScriptで作るためCoffee力の低い自分には辛いものでした。

しかし、[メルカリのエンジニアブログ](http://tech.mercari.com/entry/2015/10/15/183000)で[Real Time Messaging API](https://api.slack.com/rtm)を使ってWebSocket通信ができるというのがわかってPythonで実装しました。

とは言っても、既にPython用のライブラリ([python-rtmbot](https://github.com/slackhq/python-rtmbot))があったのをそれを使っただけです。

<!--more-->

以下python-rtmbotの使い方

## インストール

```sh
$ git clone git@github.com:slackhq/python-rtmbot.git
cd python-rtmbot
$ pip install -r requirements.txt
```

## プラグインを登録

Hubotでいう`scripts`ディレクトリが`plugin`ディレクトリになります

ここにプラグインを入れていきます。

```sh
$ mkdir plugins/repeat
$ cp doc/example-plugins/repeat.py plugins/repeat
```

## Slack上にbotを登録

[Integrations](https://mursts-jp.slack.com/services/new/bot)でbotを登録します

今はbot用に`Bots`というIntegrationsがあるんですね。

![](/post/2015/10/regist-bot.jpg)

ボット名を登録したら完了です。

![](/post/2015/10/registed-bot1.jpg)

## 設定

`SLACK_TOKEN`に上の画面でみたAPI Tokenを設定します

```sh
cp doc/example-config/rtmbot.conf .
vi rtmbot.conf
  SLACK_TOKEN: "xoxb-11111111111-222222222222222"
```

## 実行

Python3.5ではエラーになったので、2.7.10で実行しました。

```sh
$ ./rtmbot.py
```

botにダイレクトメッセージを送ると以下のようになります。

![](/post/2015/10/message-bot.jpg)

## プラグインを作成する

基本的には`plugin`ディレクトリにファイルを置くだけ

### メッセージ受信

```python
def process_message(data):
    print data
```

`process_message`を定義すると引数に↓のような情報が入った辞書が渡ってくる

```
 {u'text': u'What\u2019s your name',
  u'ts': u'1445087609.000007',
  u'user': u'U02LJFLE8',
  u'team': u'T02LJFLE6',
  u'type': u'message',
  u'channel': u'D0CLD7F5M'}
 ```

ユーザやチームは名前で入っているけど、IDから変換するのはどうやるんだろう？

### メッセージ送信

 ```python
outputs = []
outputs.append(["C12345667", "hello world"])
```

`outputs`という名のリストを作ってそこにリストを追加する

リストの1番目はチャンネル名で、2番目はメッセージ

READMEではチャンネルはIDになっているけど、名前でも送信できました。

チャンネルに向けて送信する場合は、botをチャンネルに招待するのを忘れずに

## まとめ

`Real Time Messaging API`を使うことによって言語を選ばずに、さらにはサーバを公開しないでもbotが作れるようになりました。

また、`python-rtmbot`のおかげで自前でWebSocketの実装をすることがなく感謝するばかりです。
