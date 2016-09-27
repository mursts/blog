+++
date = "2016-09-27T23:42:21+09:00"
slug = "cloud-datastore-python"
tags = ["GCP", "Datastore", "Python"]
title = "Google Cloud Datastoreにデータを登録する(Not GAE)"

+++

GAEからではなく、ローカルからPython3でCloud Datastoreにデータを登録してみました。

{{% blogcard "https://cloud.google.com/datastore/" %}}

<!--more-->

## 事前準備

### サービスアカウント作成して鍵を取得

1. [Google Cloud Platform Console](https://console.cloud.google.com/)を開く
1. `API Manager` - `認証情報`で認証情報を作成する

    1. `認証情報を作成`ボタン - `サービスアカウントキー`
    1. `サービスアカウント`で`新しいサービスアカウント`を選択(未作成の場合)
    1. アカウント名と役割を入力して、キーのタイプを選択する(jsonが推奨)

![](/post/2016/09/API-Manager1.jpg)

![](/post/2016/09/API-Manager.jpg)

作成すると認証情報のファイルがダウンロードできます。

### ライブラリの準備

`gcloud`をpip等でインストールします

```sh
$ pip install gcloud
```

## コード

### イメージ

作成するTodoリストのイメージ

- Kind
    - Todo
- Property
    - text - String
    - done - Bool
    - created - Datetime

事前準備でダウンロードしたファイルを`service_account_key.json`とする

### put

```python
#!/usr/bin/env python
# coding: utf-8

import os
from datetime import datetime
from gcloud import datastore

PROJECT_NAME = 'プロジェクト名'

def main():
    service_account_file = os.path.join(os.path.dirname(__file__), 'service_account_key.json')
    client = datastore.Client.from_service_account_json(service_account_file,
                                                        project=PROJECT_NAME)

    key = client.key('Todo')
    todo = datastore.Entity(key)
    todo.update({
        'text': '牛乳を買う',
        'done': False,
        'created': datetime.now()
    })

    client.put(todo)

if __name__ == '__main__':
    main()
```

### get

```python
#!/usr/bin/env python
# coding: utf-8

import os
from gcloud import datastore

PROJECT_NAME = 'プロジェクト名'

def main():
    service_account_file = os.path.join(os.path.dirname(__file__), 'service_account_key.json')
    client = datastore.Client.from_service_account_json(service_account_file,
                                                        project=PROJECT_NAME)

    query = client.query(kind='Todo')
    query.add_filter('done', '=', False)
    query.order = ['created']

    print([dict(entity) for entity in query.fetch()])

if __name__ == '__main__':
    main()
```

## Datastore Emulator

DatastoreへのアクセスもGAEと同じように開発環境が用意されています。

{{% blogcard "https://cloud.google.com/datastore/docs/tools/datastore-emulator" %}}

{{% blogcard "http://googlecloudplatform-japan.blogspot.jp/2016/01/google-cloud-cli.html" %}}


### エミュレータ起動

起動には[Google Cloud SDK](https://cloud.google.com/sdk/)が必要です。

```sh
$ gcloud beta emulators datastore start --project=プロジェクトID
# エミュレータがインストールされていない場合はインストールが始まります。
```


管理画面を開くには`http://localhost:xxxx/_ah/admin`にアクセスします(ポート番号は毎回違うようです)

エミュレータを実行したら`$(gcloud beta emulators datastore env-init)`を別のコンソールで実行してエミュレータにつなぐための環境変数を設定します。

あとは作成したプログラムを実行すると、エミュレータにデータが登録されます。

![](/post/2016/09/dev_appserver.jpg)
