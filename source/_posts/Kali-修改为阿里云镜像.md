title: Kali 修改为阿里云镜像
author: l1n6yun
tags:
  - kali
categories: []
date: 2019-11-16 00:00:00
---
## 简介

Kali Linux is a Debian-derived Linux distribution designed for digital forensics and penetration testing. It is maintained and funded by Offensive Security Ltd

下载地址: https://mirrors.aliyun.com/kali/

相关仓库
- Kali安装源（kali-images）：https://developer.aliyun.com/mirror/kali-images

## 配置方法

修改 `/etc/apt/sources.list` , 将相关 url 改成阿里云的源。

```
#deb https://mirrors.aliyun.com/kali kali-rolling main non-free contrib
#deb-src https://mirrors.aliyun.com/kali kali-rolling main non-free contrib
```
## 相关链接

- 官方主页: https://www.kali.org/
- 文档：https://www.kali.org/docs/