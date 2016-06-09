---

title: "MacでHubotを動かしてみた"
date: 2014-09-19
slug: "osx-hubot"
comments: true
tags: ["Hubot"]
---

Mac上にHubotが動く環境を作ってみました

<!--more-->

### node.jsとnpmをインストール

```bash
$ brew install node npm
```

後はここにかいてある通りに実行

[https://github.com/github/hubot/tree/master/docs](https://github.com/github/hubot/tree/master/docs)

### Hubotとそれに必要なcoffee-scriptをインストール

```bash
$ npm install -g hubot coffee-script
```

### Hubotを作成

```bash
$ hubot --create myhubot
```

### Hubotを実行

```bash
$ cd myhubot
$ bin/hubot
```

**エラー発生**

```bash
hubot-scripts@2.5.16 node_modules/hubot-scripts
└── redis@0.8.4

hubot@2.8.1 node_modules/hubot
├── optparse@1.0.4
├── log@1.4.0
├── scoped-http-client@0.9.8
├── coffee-script@1.6.3
└── express@3.3.4 (methods@0.0.1, range-parser@0.0.4, fresh@0.1.0, cookie-signature@1.0.1, buffer-crc32@0.2.1, cookie@0.1.0, mkdirp@0.3.5, debug@2.0.0, commander@1.2.0, send@0.1.3, connect@2.8.4)
Hubot> [Fri Sep 19 2014 21:44:53 GMT+0900 (JST)] ERROR [Error: Redis connection to localhost:6379 failed - connect ECONNREFUSED]
```

どうやらRedisが必要みたいです。

**追記**

hubot-scripts.json をいじればRedisをインストールしなくても大丈夫そうです

しかし、Redisをどこで使うかはわかっていません

```bash
$ diff hubot-scripts.json.old hubot-scripts.json
1c1
< ["redis-brain.coffee", "shipit.coffee"]
---
> ["shipit.coffee"]
```

### Redisをインストールして起動

```bash
$ brew install redis
$ redis-server --port 6379
```

### 再度Hubotを実行

```bash
$ cd myhubot
$ bin/hubot
Hubot> hubot ping
Hubot> PONG
Hubot>
```

### TODO

- herokuで実行する
- slackと連携する
