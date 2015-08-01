+++
date = "2015-08-01T23:59:23+09:00"
slug = "hubot-slack-on-raspberrypi"
tags = ["Raspberry Pi", "Hubot", "Slack"]
title = "Raspberry PiでHubot + Slackを動かす"

+++

HubotをRaspberry Piで動かしてみました。
ただ外に公開していないため参考記事のようにXMPPプロトコルを使いました。

#### 参考

* [Slack を XMPP プロトコルで Hubot と連携する - Atsushi Nagase](http://ja.ngs.io/2014/08/01/slack-hubot-xmpp/)
* [hubotをxmppを使ってslackで動かす | せかいらぼ](http://www.sekailab.com/wp/2014/09/18/hubot-xmpp-slack/)

<!--more-->

以下参考記事そのまま

---

## Hubotとその他必要な物をインストール

Node.jsは[過去の記事](/entry/2015/07/08/install-nodejs-to-raspberrypi/)を参照
```
$ npm install -g hubot coffee-script yo generator-hubot
```

## Hubotを作成

```
$ mkdir hubot
$ cd hubot
$ yo hubot
                     _____________________________  
                    /                             \
   //\              |      Extracting input for    |
  ////\    _____    |   self-replication process   |
 //////\  /_____\   \                             /
 ======= |[^_/\_]|   /----------------------------  
  |   | _|___@@__|__
  +===+/  ///     \_\
   | |_\ /// HUBOT/\\
   |___/\//      /  \\
         \      /   +---+
          \____/    |   |
           | //|    +===+
            \//      |xx|



# ・・・いろいろ聞かれるので答える・・・

```

## XMPPアダプターもインストール

```
$ npm install hubot-xmpp --save
```

## 実行スクリプトにXMPPの設定を追加

```
$ vi bin/hubot

export HUBOT_XMPP_HOST=conference.<グループ名>.xmpp.slack.com
export HUBOT_XMPP_ROOMS=general@$HUBOT_XMPP_HOST
export HUBOT_XMPP_USERNAME=<hubot用ユーザー名>@<グループ名>.xmpp.slack.com
export HUBOT_XMPP_PASSWORD=<取得したパスワード>
```

## hubot-heroku-keepaliveを削除

今回はRaspiで動かしてHerokuを起こす必要がないので削除

```
$ vi external-scripts.json

  "hubot-heroku-keepalive" #この行を削除
```

## 実行

```
$ bin/hubot -a xmpp
```

## エラー？発生

今回実行すると以下の様なメッセージが定期的にでました。

メッセージがでているけどHubotは動いているので今回はスルー

```
 [xmpp error]<iq type="error" xmlns:stream="http://etherx.jabber.org/streams"><error type="modify"><bad-request xmlns="urn:ietf:params:xml:ns:xmpp-stanzas"/><text xmlns="urn:ietf:params:xml:ns:xmpp-stanzas" xml:lang="en">Missing 'id'</text></error></iq>
```
