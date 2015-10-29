+++
date = "2015-10-30T00:31:27+09:00"
slug = "get-started-with-docker-machine-and-a-local-vm"
tags = ["Docker"]
title = "Docker Machine入門"

+++

[前回](/entry/2015/10/28/get-started-with-docker/)の記事でMacにDockerをインストールしてhello world的なことをやったけど、あれだけだとよくわからないので改めてDocker Machineに入門する

https://docs.docker.com/machine/get-started/

## Docker Machineで仮想マシンを作成する

`docker-machine create`で仮想マシン(VM)を作成する

ここではVirtualBoxに`dev`という名前で作成する

```sh
$ docker-machine create --driver virtualbox dev

Creating VirtualBox VM...
Creating SSH key...
Starting VirtualBox VM...
Starting VM...
To see how to connect Docker to this machine, run: docker-machine env dev
```

DockerデーモンがインストールされているLinuxディストリビューションをダウンロードして、VMを作って起動している。

`docker-machine ls`で確認できる

```sh
$ docker-machine ls

NAME   ACTIVE   DRIVER       STATE     URL                         SWARM
dev             virtualbox   Running   tcp://192.168.99.101:2376   
```

### Dockerを動かしてみる

```
$ docker ps

Get http:///var/run/docker.sock/v1.20/containers/json: dial unix /var/run/docker.sock: no such file or directory.
* Are you trying to connect to a TLS-enabled daemon without TLS?
* Is your docker daemon up and running?
```

Docker Machineは複数のVMを管理できるため、どのVMでDockerを実行するかを指定する必要がある。

### 環境変数を設定する

`env`コマンドで各VMの環境変数が確認できるので、これを使って指定する

```sh
$ docker-machine env dev
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.101:2376"
export DOCKER_CERT_PATH="/Users/satoshi/.docker/machine/machines/dev"
export DOCKER_MACHINE_NAME="dev"
# Run this command to configure your shell:
# eval "$(docker-machine env dev)"
```

コメントにあるようにevalを実行する

```sh
$ eval "$(docker-machine env dev)"

$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

設定する前はDockerデーモンが動いていないと怒られたけど、今回は問題なく実行できている。

## Nginxを動かす

```sh
$ docker run -d -p 8000:80 nginx

Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx

Digest: sha256:b24651e86659a5d1e4103f8c1ea49567335528281c1678697783ae7569114e1e
Status: Downloaded newer image for nginx:latest
dec39bfb13c9bffd960cde439833a727ccd70560f6dff80442182d8a45686d49
```

[前回](/entry/2015/10/28/get-started-with-docker/)のhello-worldと同じようにDocker HubからNginxのイメージをロードしている

プロセスを確認するとコンテナが作成されている

```sh
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                           NAMES
dec39bfb13c9        nginx               "nginx -g 'daemon off"   3 minutes ago       Up 3 minutes        443/tcp, 0.0.0.0:8000->80/tcp   desperate_leakey
```

`0.0.0.0:8000->80`にあるようにVMの8000番ポートがコンテナの80番にポートフォワードされている

VMのIPを調べてブラウザでアクセスする

```
$ echo $(docker-machine ip dev)
192.168.99.101
```

ハローワールド

![](/post/2015/10/nginx-on-docker.jpg)

VMを使い終わったら`stop`で停止して、再度使う場合は`start`で起動する

```sh
$ docker-machine stop dev
$ docker-machine start dev
```

## まとめ

前回は、`Docker Quickstart Terminal`を使ったのでDocker Machineって何って感じだったけど、これで多少は理解できた気がします。
