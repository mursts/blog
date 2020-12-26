+++
date = "2020-12-26"
slug = "raspi-bme680"
title = "Raspberry PiとBME680を使って部屋の環境データを取得する"
tags = ["raspberry-pi"]
isCJKLanguage = true
+++

部屋の環境データを取るためにBME680 Breakoutを購入したので動かすためのメモ  

{{% blogcard "https://shop.pimoroni.com/products/bme680-breakout" %}}

{{% blogcard "https://shop.pimoroni.com/products/breakout-garden-mini-i2c" %}}

インストールについてはGitHubのレポジトリの通り

{{% blogcard "https://github.com/pimoroni/bme680-python#manual-install" %}}

```sh
$ sudo apt install python3-venv
$ python3 -m venv .venv
$ source .venv/bin/activate
$ pip install bme680

$ wget -O read-all.py "https://raw.githubusercontent.com/pimoroni/bme680-python/master/examples/read-all.py"
$ chmod +x read-all.py

$ python read-all.py
read-all.py - Displays temperature, pressure, humidity, and gas.

Press Ctrl+C to exit!


Traceback (most recent call last):
  File "read-all.py", line 13, in <module>
    sensor = bme680.BME680(bme680.I2C_ADDR_PRIMARY)
  File "/home/mursts/room_sensor/.venv/lib/python3.7/site-packages/bme680/__init__.py", line 22, in __init__
    import smbus
ModuleNotFoundError: No module named 'smbus'
```

smbusが必要なのでインストールして再実行

```sh
$ pip install smbus
$ python read-all.py
read-all.py - Displays temperature, pressure, humidity, and gas.

Press Ctrl+C to exit!


Traceback (most recent call last):
  File "read-all.py", line 13, in <module>
    sensor = bme680.BME680(bme680.I2C_ADDR_PRIMARY)
  File "/home/mursts/room_sensor/.venv/lib/python3.7/site-packages/bme680/__init__.py", line 23, in __init__
    self._i2c = smbus.SMBus(1)
FileNotFoundError: [Errno 2] No such file or directory

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "read-all.py", line 15, in <module>
    sensor = bme680.BME680(bme680.I2C_ADDR_SECONDARY)
  File "/home/mursts/room_sensor/.venv/lib/python3.7/site-packages/bme680/__init__.py", line 23, in __init__
    self._i2c = smbus.SMBus(1)
FileNotFoundError: [Errno 2] No such file or directory
```

ぐぐるとI2Cを有効にする必要があるっぽい

{{% blogcard "https://uenoshin.hatenablog.com/entry/2019/07/12/223203" %}}

I2Cを有効化して再々実行

```sh
$ sudo raspi-config nonint do_i2c 0
$ python read-all.py
read-all.py - Displays temperature, pressure, humidity, and gas.

Press Ctrl+C to exit!


Traceback (most recent call last):
  File "read-all.py", line 13, in <module>
    sensor = bme680.BME680(bme680.I2C_ADDR_PRIMARY)
  File "/home/mursts/room_sensor/.venv/lib/python3.7/site-packages/bme680/__init__.py", line 25, in __init__
    self.chip_id = self._get_regs(CHIP_ID_ADDR, 1)
  File "/home/mursts/room_sensor/.venv/lib/python3.7/site-packages/bme680/__init__.py", line 296, in _get_regs
    return self._i2c.read_byte_data(self.i2c_addr, register)
OSError: [Errno 121] Remote I/O error

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "read-all.py", line 15, in <module>
    sensor = bme680.BME680(bme680.I2C_ADDR_SECONDARY)
  File "/home/mursts/room_sensor/.venv/lib/python3.7/site-packages/bme680/__init__.py", line 25, in __init__
    self.chip_id = self._get_regs(CHIP_ID_ADDR, 1)
  File "/home/mursts/room_sensor/.venv/lib/python3.7/site-packages/bme680/__init__.py", line 296, in _get_regs
    return self._i2c.read_byte_data(self.i2c_addr, register)
OSError: [Errno 121] Remote I/O error
```

今度はI/O Error。。。  
I/O Errorについては、単純にBME680 Breakoutを前後ろ逆に挿していただけでした  
正しい向きで指したら成功しました  

```sh
$ python read-all.py
read-all.py - Displays temperature, pressure, humidity, and gas.

Press Ctrl+C to exit!


Calibration data:
par_gh1: -44
par_gh2: -6482
par_gh3: 18
par_h1: 772
par_h2: 1011
par_h3: 0
par_h4: 45
par_h5: 20
par_h6: 120
par_h7: -100
par_p1: 36381
par_p10: 30
par_p2: -10419
par_p3: 88
par_p4: 8475
par_p5: -172
par_p6: 30
par_p7: 38
par_p8: -2221
par_p9: -2771
par_t1: 26060
par_t2: 26271
par_t3: 3
range_sw_err: 0
res_heat_range: 1
res_heat_val: 43
t_fine: 150787
```

センサーの値はとれたから次は可視化


