+++
date = "2016-04-02T01:01:44+09:00"
slug = "2016-04-02-ffmpeg-video-split"
tags = ["FFmpeg"]
title = "FFmpegで動画を指定時間で切り取る"

+++

```sh
$ ffmpeg -i input.mp4 -vcodec copy -acodec copy -ss 00:01:00 -to 00:03:00 output.mp4
```

`ss`で開始時間を指定して、`to`で終了時間を指定する。この例では1〜3分の計2分の動画にします。
