+++
date = "2022-06-08"
slug = "hello-seeed-xiao-rp2040"
title = "Seeed XIAO RP2040でLチカをする"
tags = ["xiaorp2040", "circuitpython"]
isCJKLanguage = true
+++

公式サイトを参考にXIAO RP2040とCircuitPythonでLチカするまでのメモ

{{% blogcard "https://wiki.seeedstudio.com/XIAO-RP2040-with-CircuitPython/" %}}


## ファームウェアをアップデート

CircuitPythonの環境を作るめにファームウェアをアップデートする

### ファームウェアをダウンロード

https://files.seeedstudio.com/wiki/XIAO-RP2040/res/XIAO-RP2040-CircuitPython.uf2

### XIAO RP2040をブートローダーモードで起動する

BOOTボタンを押しながら自分のPCとUSB Type-Cケーブルで接続する。しばらくするとPCにXIAO RP2040が`RP1-RP2`としてマウントされる

![](/post/2022/06/08/_CircuitPython-Install1.png)

### ファームウェアをコピーする

マウントされたドライブにダウンロードしたファームウェアをコピーする。ドライブが`CIRCUITPY`になる

![](/post/2022/06/08/_CircuitPython-Install2.png)

ここまでできたら`code.py`にコードを書いていく


## コードを書く

Thonny(日本語環境)を使って書く

### Interpreterの設定

[実行] - [Select Interpreter] の [インタプリタ]タブで`CircuitPython(generic)`を選択する

## code.pyを開く

[ファイル] - [Open]を選択して `Where to open from?`のダイヤログで`CircuitPython device`を選択して`code.py`を開く

## code.py

```python
import time
import board
import digitalio

led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT

while True:
    led.value = True
    time.sleep(0.5)
    led.value = False
    time.sleep(0.5)
```

実行するとUSER LEDが青色に0.5秒間隔で点滅する
