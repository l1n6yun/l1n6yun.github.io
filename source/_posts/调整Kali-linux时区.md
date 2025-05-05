title: 调整Kali linux时区
author: l1n6yun
tags: 
 - kali-linux
 - 时区设置
 - timedatectl
categories: []
date: 2019-11-17 00:00:00
---
目的：渗透测试和安全审计中需要kali linux的系统时间与实际时间同步。

命令：`sudo timedatectl set-timezone "Asia/Shanghai"`

使用命令 `timedatectl` 查看当前时区等信息