+++
date = "2016-09-24T23:41:50+09:00"
slug = "requests-on-gaepy"
tags = ["googleappengine", "python"]
title = "GAE/Pyでrequestsを使う"
+++

今までGAEから他サービスへhttpのリクエストを投げる場合はurlfetchを使っていました(それしか使えなかった？)。
最近マニュアルを見たらurlfetch以外にもurllib2やrequestsを使えるとあったので、requestsでリクエストする方法の紹介します。

<!--more-->

[Issuing HTTP(S) Requests](https://cloud.google.com/appengine/docs/python/issue-requests)

## 必要なもの

- requests
- requests_toolbelt

2つをpipでそのままインストールするのではなく、 GEAで使う場合は`lib`フォルダなんかににライブラリを入れます

[Requesting a library] (https://cloud.google.com/appengine/docs/python/tools/using-libraries-python-27#installing_a_library)

```sh
$ pip install requests requests_toolbelt -t lib
```

## 使いかた

```python
import requests
import requests_toolbelt.adapters.appengine

# requestsにモンキーパッチを当てて内部でurlfetchを使うようにします
requests_toolbelt.adapters.appengine.monkeypatch()
```

あとは通常のrequestsと同じように使えます

```python
class MainHandler(webapp2.RequestHandler):
    def get(self):
        url = 'http://example.com'
        r = requests.get(url)
        self.response.write(r.content)
```

requestsが使えるようになると`requests-oauthlib`も使えるので、OAuth系の処理を行うときに比較的楽に実装できるようになります。
