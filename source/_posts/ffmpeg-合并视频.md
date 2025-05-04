title: ffmpeg 合并视频
author: l1n6yun
tags: 
 - ffmpeg
 - 视频合并
 - shellscripts
categories: []
date: 2022-10-03 22:49:47
---
```shell
ffmpeg -f concat -i filelist.txt -c copy -y FBF73ED7.mp4
```

filelist.txt

```
file 'FBF73ED7.p701.1.mp4'
file 'FBF73ED7.p701.2.mp4'
file 'FBF73ED7.p701.3.mp4'
file 'FBF73ED7.p701.4.mp4'
file 'FBF73ED7.p701.5.mp4'
file 'FBF73ED7.p701.6.mp4'
file 'FBF73ED7.p701.7.mp4'
file 'FBF73ED7.p701.8.mp4'
file 'FBF73ED7.p701.9.mp4'
file 'FBF73ED7.p701.10.mp4'
file 'FBF73ED7.p701.11.mp4'
file 'FBF73ED7.p701.12.mp4'
file 'FBF73ED7.p701.13.mp4'
file 'FBF73ED7.p701.14.mp4'
file 'FBF73ED7.p701.15.mp4'
file 'FBF73ED7.p701.16.mp4'
file 'FBF73ED7.p701.17.mp4'
file 'FBF73ED7.p701.18.mp4'
file 'FBF73ED7.p701.19.mp4'
file 'FBF73ED7.p701.20.mp4'
file 'FBF73ED7.p701.21.mp4'
file 'FBF73ED7.p701.22.mp4'
file 'FBF73ED7.p701.23.mp4'
file 'FBF73ED7.p701.24.mp4'
file 'FBF73ED7.p701.25.mp4'
file 'FBF73ED7.p701.26.mp4'
file 'FBF73ED7.p701.27.mp4'
file 'FBF73ED7.p701.28.mp4'
file 'FBF73ED7.p701.29.mp4'
file 'FBF73ED7.p701.30.mp4'
```
