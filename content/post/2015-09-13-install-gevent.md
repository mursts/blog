+++
date = "2015-09-13T23:49:55+09:00"
slug = "install-gevent"
tags = ["Python"]
title = "geventをインストールする"

+++

## 環境

* Python 3.4.3

単純にpipインストールした場合、

```
$ pip install gevent
```

以下のエラーでインストールに失敗しました。

```
TypeError: unsupported operand type(s) for >>: 'builtin_function_or_method' and '_io.TextIOWrapper'
```

<!--more-->

[pypi](https://pypi.python.org/pypi/gevent)を見るとstable版はPython2系のみで3系はベータ版のみ対応でした。

なのでバージョンを指定してインストールするとインストールは成功です

```
$ pip install gevent==1.1b4
```
