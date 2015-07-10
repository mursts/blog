---

title: "Ubuntu12.04にPostgres9.3をインストールする"
date: 2014-01-28
comments: true
tags: ["Ubuntu", "PostgreSQL"]
---

Ubuntu12.04に最新版のPostgres9.3(2014/1/19現在)をインストールしました。
その手順をメモ。

<!--more-->

## 環境
```bash
$ lsb-release -a

No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 12.04.4 LTS
Release:        12.04
Codename:       precise
```

## sources.list作成

```bash
$ cat /etc/apt/sources.list.d/postgres9.3.list
deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main
```

precise-pgdgの **precise** はUbuntuのバージョンによって変える{環境}のCodename参照

## インストール

```bash
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
$ sudo aptitude update
$ sudo aptitude install postgresql-9.3

$ psql --version
psql (PostgreSQL) 9.3.2
```

## DBを初期化する

作成するDBをデフォルトでUTF8にするためDBを初期化

DB作成先はデフォルトの場所を指定

```bash
$ /usr/lib/postgresql/9.3/bin/initdb --encoding=UTF-8 --no-locale -D /var/lib/postgresql/9.3/main
```

### ユーザ作成

postgresユーザにスイッチしてから作業

```bash
$ createuser --interactive username
Shall the new role be a superuser? (y/n)
Shall the new role be allowed to create databases? (y/n)
Shall the new role be allowed to create more new roles? (y/n) 
```

## DBを作成

```bash
$ createdb testdb

$ psql testdb

testdb=> \l
                             List of databases
   Name    |  Owner   | Encoding | Collate | Ctype |   Access privileges
-----------+----------+----------+---------+-------+-----------------------
 postgres  | postgres | UTF8     | C       | C     |
 template0 | postgres | UTF8     | C       | C     | =c/postgres          +
           |          |          |         |       | postgres=CTc/postgres
 template1 | postgres | UTF8     | C       | C     | =c/postgres          +
           |          |          |         |       | postgres=CTc/postgres
 testdb    | mursts   | UTF8     | C       | C     |
(4 rows)
```
