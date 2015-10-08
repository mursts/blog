+++
date = "2015-10-08T19:11:51+09:00"
slug = "osx-invalid-ipv6"
tags = ["OSX"]
title = "OSXでIPv6を無効にする"

+++

外でインターネットにつなぐ場合は、docomoのL-03D(LTE)を使っています。
しかし、OSのバージョンアップ(→Yosemite)したせいか１Mくらいしか速度がでません。(Lionくらいの時ははそれなりにでていた)

IPv6を無効にするとネットワークが安定して高速化するという話なので無効にしてみました。

<!--more-->

* ネットワークインターフェースを表示して対象を探す

```
$ networksetup -listnetworkserviceorder

(1) Bluetooth DUN
(Hardware Port: Bluetooth DUN, Device: Bluetooth-Modem)

(2) docomo L03D
(Hardware Port: docomo L03D, Device: en3)

(3) Wi-Fi
(Hardware Port: Wi-Fi, Device: en0)

(4) Bluetooth PAN
(Hardware Port: Bluetooth PAN, Device: en2)

(5) Thunderbolt Bridge
(Hardware Port: Thunderbolt Bridge, Device: bridge0)
```

* 指定のネットワークのIPv6を無効にする

ここでは、L-03Dで無効にする。

ネットワーク名にスペースがある場合は"で囲む
```
$ sudo networksetup -setv6off "docomo L03D"
```

これでIPv6を無効にした結果、通信速度はまったく変わらず遅いままでした。

販売から３年くらい経って新しいOSに対応してくれそうもないので、そろそろL-03Dの引退を考える必要がありそうです。
