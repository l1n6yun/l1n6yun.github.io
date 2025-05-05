title: >-
  win10 下 git 运行出现 fatal: open /dev/null or dup failed: No such file or
  directory
description: 生命在于折腾，又把博客折腾到Hexo了。给Hexo点赞。
date: 2018-06-19 15:51:56
categories:
tags: 
 - git
 - windows10
 - null.sys
 - 系统修复
---
## 方法一

以管理员运行 `CMD` ，输入命令 `sfc /scannow` 进行修复操作，然后重启就可以用了。

## 方法二

下载 [null.sys](/file/null.rar) 文件替换到 `C:\Windows\System32\drivers\null.sys`
