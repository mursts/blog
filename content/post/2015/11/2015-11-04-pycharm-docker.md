+++
date = "2015-11-04T00:04:08+09:00"
slug = "pycharm-docker-integration"
tags = ["Python", "Docker", "PyCharm"]
title = "DockerコンテナのPythonをPyCharmから実行する"

+++

先日リリースされた[PyCharm 5](https://www.jetbrains.com/pycharm/)でDockerコンテナにあるPythonを実行できるようになったの早速試してみました。

<!--more-->

## Pythonイメージをダウンロード

今までの記事では、`docker run`で作成と実行を同時にやっていたけど、今回はイメージを持ってくるだけなので`docker pull`になる

```sh
$ docker pull python:latest

$ docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
python              latest              6366b0d032f2        10 days ago         688.8 MB
```

## PyCharmから実行する

Interpreterの歯車のところで`Add Remote`を選択

![](/post/2015/11/pycharm-docker1.jpg)

`Docker`を選択すると`Machine name`のところにVMの一覧が出るので選択する。`Image name`でダウンロードしたPythonイメージを選択する

![](/post/2015/11/pycharm-docker2.jpg)

すると。DockerのInterpreterが作成される。

![](/post/2015/11/pycharm-docker3.jpg)

あとは、HelloWorldをやってみる。`os.environ['HOME']`の値もローカルのパスではなく、VMのパスになっていそうなので、コンテナ上で実行されているのがわかる。

![](/post/2015/11/pycharm-docker4.jpg)

`docker ps`を見るとPyCharmで作成されてっぽコンテナがありました。

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS               NAMES
b97ba15efa17        025e6f1f4226        "/bin/sh"                19 minutes ago      Created                                     pycharm_helpers_PY-143.589
```

リモート上のスクリプトを実行するのにターミナルでリモートにつないから実行するのではなく、IDEから実行できるのですごく楽になりました。

Dockerだけじゃなく、Raspiにあるスクリプトを実行するのにも重宝しそうです。
