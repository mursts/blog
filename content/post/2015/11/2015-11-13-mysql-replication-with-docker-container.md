+++
date = "2015-11-13T23:36:03+09:00"
slug = "mysql-replication-with-docker-container"
tags = ["MySQL", "Docker"]
title = "Dockerコンテナを使ってMySQLのレプリケーションをやってみる"

+++

[前回の記事](/entry/2015/11/01/mysql-on-docker-container/)の続きです。

2つのホストにそれぞれMySQLコンテナを作成してレプリケーションをしてみました。
今回はDockerでやっただけでVPS等でもやり方は一緒のはずです

レプリケーションの設定はOracle主催のMySQLセミナーで頂いた資料を元にしています。

## MySQLの準備

docker-machineでホストから作成する

### ホストの作成

`master`と`slave`という名前でホストを作成します

```sh
$ docker-machine create --driver virtualbox master
$ docker-machine create --driver virtualbox slave

$ docker-machine ls
NAME     ACTIVE   DRIVER       STATE     URL                         SWARM
master            virtualbox   Running   tcp://192.168.99.103:2376   
slave             virtualbox   Running   tcp://192.168.99.104:2376
```

### コンテナの作成

#### `master`ホスト

```sh
$ eval "$(docker-machine env master)"
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 13306:3306 -d mysql:latest --server-id=1001 --log-bin=mysql-bin --gtid-mode=on --enforce-gtid-consistency=on --log-slave-updates

# 接続確認
$ mysql -h "$(docker-machine ip master)" -u root -P 13306  -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.9 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>

```

#### `slave`ホスト

```sh
$ eval "$(docker-machine env slave)"
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 13306:3306 -d mysql:latest --server-id=1002 --log-bin=mysql-bin --gtid-mode=on --enforce-gtid-consistency=on --log-slave-updates --read_only
```

## レプリケーションユーザの作成

masterのMySQLに接続して作成する

```sh
mysql> CREATE USER 'repl'@'localhost' IDENTIFIED BY 'repl';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'localhost';
```

## レプリケーション実行

slave側で作業する

```sh
mysql> CHANGE MASTER TO MASTER_HOST='192.168.99.103',
    -> MASTER_USER='repl',
    -> MASTER_PASSWORD='repl',
    -> MASTER_AUTO_POSITION=1,
    -> MASTER_PORT=13306;

mysql> START SLAVE;
```

## レプリケーションの確認

master側でユーザを作成して、slaveにもできるかを確認します。

masterの作成前の状態

```sh
mysql> use mysql;
mysql> select user From user;
+-------+
| user  |
+-------+
| root  |
| repl  |
+-------+
```

master側で`repl2`ユーザを作成する

```sh
mysql >CREATE USER 'repl2'@'localhost' IDENTIFIED BY 'repl';

mysql> select user From user;
+-------+
| user  |
+-------+
| root  |
| repl  |
| repl2 |
+-------+
```

レプリケーションが設定できていればslave側でもuserテーブルに`repl2`ユーザが作成されているはずです。

今回は検証でユーザ作っだけの確認ですが、Wordpressでブログを作成してそのDBためしてみるともっとレプリケーションが実感できると思います。
