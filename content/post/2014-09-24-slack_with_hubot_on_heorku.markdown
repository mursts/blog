---

title: "SlackとHubotを連携してHerokuにデプロイ"
date: 2014-09-24
slug: "slack-hubot-heroku"
comments: true
tags: ["Slack", "Hubot", "Heroku"]
---

[前回](http://blog.mursts.jp/2014/09/19/start_hubot_on_mac.html) の続き

参考サイト:[https://github.com/tinyspeck/hubot-slack](https://github.com/tinyspeck/hubot-slack)

<!--more-->

## Hubotを作る

- Hubotを作る

```bash
$ hubot --create mybot
$ cd mybot
```

- アダプターをインストール

```bash
$ npm install hubot-slack --save
```

package.json のdependenciesにhubot-slackが追加される

- Redisを使わないように hubot-scripts.json を更新

```bash
["shipit.coffee"]
```

## Herokuにデプロイ

- Procfileを更新する

```bash
web: bin/hubot --adapter slack
```

- heroku toolbeltをインストールする

macなら

```bash
$ brew install heroku-toolbelt
```

- Herokuのプロジェクト作成

```bash
$ heroku create my-bot
```

- SlackのIntegrationを追加して必要なトークンを取得

```bash
https://<your-team>.slack.com/services/new/hubot
```

- トークンを設定

```bash
$ heroku config:add HEROKU_URL=http://<your-project>.herokuapp.com
$ heroku config:add HUBOT_SLACK_TOKEN=*****************
$ heroku config:add HUBOT_SLACK_TEAM=myteam
$ heroku config:add HUBOT_SLACK_BOTNAME=slackbot
```

- タイムゾーンの設定

```bash
$ heroku config:add TZ=Asia/Tokyo
```

- デプロイ

```bash
$ git init
$ git add .
$ git commit -m "first commit"
$ git remote add heroku git@heroku.com:<your-project>.git
$ git push heroku master
$ heroku ps:scale web=1
```

これでHubotが反応してくれれば終了

![](/post/slack_hubot.jpg)

## Hubotが反応しない場合

```bash
bash: bin/hubot: No such file or directory
```

binディレクトリがcommitされていますか？

私の場合、グローバルの.gitignoreでbinディレクトリを除外していたためbinディレクトリがcommitできずに上記のエラーがでていました。
