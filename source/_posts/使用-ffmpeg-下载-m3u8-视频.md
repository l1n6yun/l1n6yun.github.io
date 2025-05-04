title: 使用 ffmpeg 下载 m3u8 视频
author: l1n6yun
tags: 
 - ffmpeg
 - m3u8
 - mp4
 - 视频下载
categories: []
date: 2022-10-05 16:29:36
---
下载视频，并将m3u8格式转为mp4格式

```
ffmpeg -i https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8 playlist.mp4
或者
ffmpeg -i https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8 -c copy playlist.mp4
```

下载中。。。由于视频很大，下载需要很长长长时间(1个G的视频可能需下载几小时…)。
 可以通过如下指令进行下载提速（下载速度大约能提升到几到十几分钟，很棒了哦，起码比百度云快）：

```
ffmpeg -i https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8 -c copy -bsf:a aac_adtstoasc playlist_1.mp4
```