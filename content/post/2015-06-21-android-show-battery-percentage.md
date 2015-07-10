+++
date = "2015-06-21T02:44:52+09:00"
slug = "android-show-battery-percentage"
title = "Androidのバッテリー残量をパーセンテージで表示する"
tags = ["Android"]
+++

```bash
adb shell content insert --uri content://settings/system --bind name:s:status_bar_show_battery_percent --bind value:i:1

adb reboot
```

### 参考

[Android 4.4 Includes Support For Battery Percentage In The Status Bar – It's Not Ideal, But Here's How To Enable It (No Root Needed)](http://www.androidpolice.com/2013/11/11/android-4-4-includes-support-for-battery-percent-in-the-status-bar-its-not-ideal-but-heres-how-to-enable-it-without-root/)
