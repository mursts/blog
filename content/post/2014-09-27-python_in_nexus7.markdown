---

title: "Nexus7にPythonが動く環境を作る"
date: 2014-09-27
comments: true
tags: ["Nexus7", "Android", "Python"]
---

SL4A(Scripting Layer for Android)をインストールする。おまけでpipもインストールする

タイトルはNexus7だけどAndoridならなんでもいいはず。

<!--more-->

## SL4Aのインストール

[SL4A公式](https://code.google.com/p/android-scripting/)
からsl4a\_r6.apkをダウンロード(2013.9時点での最新)してインストールする

## python-for-android(Py4A)をインストール

[Py4A公式](https://code.google.com/p/python-for-android/downloads/list?q=label:Featured)
から、Python3ForAndroid\_r6.apkをダウンロードしてインストール

## Python3.2をインストール

ランチャーにある `Python3 for Android` を起動する。

`INSTALL` ボタンをタップしてPython3.2をインストールする。

## pipをインストールする

2ファイルを `/sdcard/sl4a/scripts` にダウンロードする。

- [distribute\_setup.py](http://python-distribute.org/distribute_setup.py)
- [get-pip.py](https://raw.github.com/pypa/pip/master/contrib/get-pip.py)

ダウンロード後SL4Aから起動する

## モジュールをインストール

SL4Aの `pip_console.py` を起動して

```bash
pip install パッケージ名
```

を実行する
