+++
date = "2016-10-09T22:19:52+09:00"
slug = "gae-cloud-shell"
title = "Cloud ShellでGAEのHello World"
tags = [
  "GCP", "GoogleAppEngine"
]

+++

Cloud Shellの話を聞いて、早速GAEのHello Worldをやってみました。

{{% blogcard "https://cloud.google.com/shell/docs/" %}}

<!--more-->

## Cloud Shellを有効にする

右上のシェルっぽいボタンを押して有効化します。

![](/post/2016/10/activate-cloud-shell.jpg)

その後下記のようなコマンドが実行されました。
しかし、test-project.appspot.comは404でみつかりません。

```sh
$ gcloud compute instances list
$ git clone https://github.com/GoogleCloud/appengine-example.git
$ cd appengine-example
$ appcfg.py -A test-project update app.yaml
```

test-projectは気にせず`Cloud Shellの起動`を押して起動します

![](/post/2016/10/activate-cloud-shell3.jpg)

そうするとブラウザ上でシェルのような画面が表示されます。

![](/post/2016/10/activate-cloud-shell4.jpg)

デフォルトで`gcloud`だったり、`Python`はインストールされています

![](/post/2016/10/cloud-shell-gcloud.jpg)

![](/post/2016/10/cloud-shell-python.jpg)

他にもリンク先にあるようなものがインストール済みです。

https://cloud.google.com/shell/docs/features#tools

## GAEのソースを書く

Cloud Shell上でコードを書く場合は、ファイルをアップロードしたりコードエディタ上で編集します。

![](/post/2016/10/edit-code.jpg)

ここではコードエディターを使ってコードを書きます

左のツリーを右クリックすると、フォルダやファイルを追加できます。

![](/post/2016/10/code-editor.jpg)

`hello-gae`フォルダを追加して、その下に`app.yaml`と`main.py`を追加しています。

![](/post/2016/10/add-code.jpg)

app.yaml

```javascript
runtime: python27
api_version: 1
threadsafe: yes

handlers:
- url: .*
  script: main.app

libraries:
- name: webapp2
  version: "2.5.2"
```

main.py

```python
# coding: utf-8

import webapp2

class MainHandler(webapp2.RequestHandler):
    def get(self):
        self.response.write('Hello Cloud Shell')


app = webapp2.WSGIApplication([
    ('/', MainHandler)
], debug=True)
```

## 開発サーバを起動する

コードが準備できたらコーンソールの方でコマンドを入力します。

```sh
$ cd hello-gae
$ dev_appsever.py . #アンダーバーが入力できずにコピペしました
```

![](/post/2016/10/appserver.jpg)

開発サーバが起動したら左上にある`ウェブでプレビュー` - `ポート上でプレビュー 8080`をクリックすると起動します。

![](/post/2016/10/preview.jpg)

## デプロイ

gcloudコマンドでデプロイします

```sh
$ gcloud app deploy --project {ApplicationID} # {ApplicationID}は各自のアプリケーションIDに置き換える
```

> gcloudでデプロイする場合は、app.yamlにApplicationIDやVersionがあるとエラーになりました。

デプロイが完了したら http://{ApplicationID}.appspot.com にアクセスして確認します。

以上で、Cloud Shellを使ってのGAEのHello Worldが完了です。

## 所感

Cloud Shellは手元にCloud SDKなどが用意できない場合には重宝しそうです。

次回の[GCPUG Nagoya](http://gcpug-nagoya.connpass.com/event/42064/)ではCloud Shellを使ってのハンズオンもいいかなと思いました。

{{% blogcard "http://gcpug-nagoya.connpass.com/event/42064/" %}}
