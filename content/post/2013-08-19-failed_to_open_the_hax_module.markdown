---

title: "Androidのエミュレータが起動できなくなった"
date: 2013-08-19
slug: "android-emulator-not-startup"
comments: true
tags: ["Android"]
---

久しぶりにAndroidのエミュレータを起動したらエラーメッセージがでて起動できなくなっていました

```
[2013-03-31 00:02:54 - Emulator] HAX is not working and emulator runs in emulation mode
[2013-03-31 00:02:54 - Emulator] emulator: Failed to open the hax module
```

<!--more-->

HAXが何かわからなかったので、ググったら公式サイトに書いてありました。

[Configuring VM Acceleration on
Mac](http://developer.android.com/intl/ja/tools/devices/emulator.html#accel-vm)

> Virtual machine acceleration on a Mac requires the installation of the
> Intel Hardware Accelerated Execution Manager (Intel HAXM) kernel
> extension to allow the Android emulator to make use of CPU
> virtualization extensions.

ドライバをインストールしないといけないみたいです。

"android-sdk/extras/intel/Hardware\_Accelerated\_Execution\_Manager/IntelHAXM.dmg"を開いてIntelHAXM.mpkgを実行していけばインストールが完了。

その後エミュレータは問題なく起動しました。
