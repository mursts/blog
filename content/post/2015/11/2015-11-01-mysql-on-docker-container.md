+++
date = "2015-11-01T01:31:28+09:00"
slug = "mysql-on-docker-container"
tags = ["Docker", "MySQL"]
title = "DokcerコンテナでMySQLを動かしてみる"

+++

[前回](/entry/2015/10/30/get-started-with-docker-machine-and-a-local-vm/)の記事でDocker Machineを触ったところで、次はMySQLに挑戦してみました。

Docker Hubの公式のMySQLリポジトリがあって、詳細に一通りの説明がありました。

[Docker Hub - mysql](https://hub.docker.com/_/mysql/)

<!--more-->

## MySQLのバージョン

2015/11/01現在では3つのバージョンがあり、最新は5.7です。

- `5.5.46`, `5.5`
- `5.6.27`, `5.6`
- `5.7.9`, `5.7`, `5`, `latest`

## MySQLイメージをロードして起動する

```sh
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:latest
```

### オプション

- `--name mysql`
 - コンテナ名
- `MYSQL_ROOT_PASSWORD=my-secret-pw`
 - rootパスワードを指定する
- `mysql:latest`
 - MySQLとそのバージョンを指定
- '-d'
 - バックグラウンドで実行


 他にも

- `MYSQL_DATABASE`
 - イメージの起動時にデータベースを作成する
- `MYSQL_USER, MYSQL_PASSWORD`
 - 同じく起動時にユーザを作成
 - `MYSQL_DATABASE`と併用？
- `MYSQL_ALLOW_EMPTY_PASSWORD`

が指定できる

### ロードの確認

```sh
$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
c3026c2da6c3        mysql:latest        "/entrypoint.sh mysql"   29 seconds ago      Up 28 seconds       3306/tcp            mysql
```

### MySQLの起動確認

```sh
$ docker exec -it mysql bash # mysqlは作成したコンテナ名

$ mysql --version                                            
mysql  Ver 14.14 Distrib 5.7.9, for Linux (x86_64) using  EditLine wrapper

$ mysql -u root -p      
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.9 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

## ローカルマシンからコンテナのMｙSQLに接続

```sh
# ローカルから
$ echo $(docker-machine ip dev)
192.168.99.101
$ mysql -h 192.168.99.101 -uroot -p
Enter password:
ERROR 2003 (HY000): Can't connect to MySQL server on '192.168.99.101' (61)
```

接続できないのVMの3306ポートで動いているわけじゃなく、コンテナのポートで起動しているからだと思う。こういうときはポートフォワードしてやれば良さそう

`docker run`で作成したポートの変更方法がわからなかったので一旦削除して作りなおしました。

```sh
# 一旦停止してコンテナを削除
$ docker stop mysql
$ docker rmi -f mysql #依存コンテナを全て削除

# ポートを設定して再度作成
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 13306:3306 -d mysql:latest

# ローカルからポートを指定して接続
$ mysql -h 192.168.99.101 -uroot -P 13306 -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.9 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

今度は接続できました。

これでMySQLへの接続ができたので、次は最大の目的のコンテナで起動した２つのMy
SQLのレプリケーションを試してみようと思います。
