---
title:       "Pixelbook GoにDockerの環境を作成する"
subtitle:    ""
description: ""
date:        2020-07-09
author:      ""
image:       ""
tags:        ["docker", "chromebook", "pixelbookgo"]
#categories:  ["Tech" ]
---

Pixelbook Goで開発ができるようにDockerの環境を作成する

## 環境

Pixelbook Goの中で動いているLinux

```sh
$ uname -a 
Linux penguin 4.19.113-08528-g5803a1c7e9f9 #1 SMP PREEMPT Thu Apr 2 15:21:14 PDT 2020 x86_64 GNU/Linux
```

## Dockerインストール

公式ドキュメントどおりにインストールする

[Install Docker Engine on Debian](https://docs.docker.com/engine/install/debian/)

インストールコマンド

```sh
$ sudo apt update
$ sudo apt install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
OK
$ sudo apt-key fingerprint 0EBFCD88
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
$ sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/debian \
    $(lsb_release -cs) \
    stable"

$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io
```    

Hello World

```sh
$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:d58e752213a51785838f9eed2b7a498ffa1cb3aa7f946dda11af39286c3db9a9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
```

## sudo無しでDockerを実行する

やり方はdockerグループを作って自ユーザを追加する  
[Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/)

```sh
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
# 変更を適用
$ newgrp docker

# sudoなしでHellow World
$ docker run hello-world
Hello from Docker!
```

## Nginxも動かしてみる

ChromeOS側からLinuxのDocker内にアクセスできるか確認する

```sh
$ docker run -p 8080:80 -d nginx
```

chromeで確認
![](/post/2020/07/09/hello-nginx.png)
