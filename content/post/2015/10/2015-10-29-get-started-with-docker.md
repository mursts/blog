+++
date = "2015-10-28T23:42:09+09:00"
slug = "get-started-with-docker"
tags = ["Docker"]
title = "DockerをMacにインストールする"

+++

[マニュアル](http://docs.docker.com/mac/started/)を見ながらMacにDockerを入れてみた。

## インストール

http://docs.docker.com/mac/step_one/

Docker Toolboxをインストールする

> Homebrewで入れる場合は`docker` `docker-machine` `docker-compose`あたりを入れればいいと思うけど、初めて触るDockerなので今回はToolboxからインストールしました。

Macの場合はOSX 10.8、Windowsは7以降が必要

Docker Toolboxには次のバイナリが含まれる

- docker-machine binary
- docker
- docker-compose
- Kitematic
- Docker command-line
- Oracle VM VirtualBox

[https://www.docker.com/docker-toolbox](https://www.docker.com/docker-toolbox)からpkgファイルをダウンロード

あとはボタンをポチポチしてインストールが完了

## Docker起動

インストール完了後に下の画面がでるので、`Docker Quickstart Terminal`を押す

![](/post/2015/10/DockerToolbox.jpg)

ターミナルが開いて次の画面が表示される

![](/post/2015/10/hello-docker.jpg)

> docker is configured to use the default machine with IP 192.168.99.100
> For help getting started, check out the docs at https://docs.docker.com

デフォルト`192.168.99.100`で仮想マシンが起動するように設定されたっぽい

VirtualBoxを見ると`default`というイメージができている

## コンテナの作成

```
$ docker run hello-world
```

`hello-world`イメージを持っているかチェックして、なければ[Docker Hub](https://hub.docker.com/)からダウンロード。そしてイメージをコンテナにロードする

```
$ docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
hello-world         latest              0a6ba66e537a        2 weeks ago         960 B
```
`docker images`コマンドを実行して確認すると、`hello-world`イメージができている。

ただこれをどうするか、どう使うかが見えない。

続きはまた後日
