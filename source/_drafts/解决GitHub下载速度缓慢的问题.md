---
title: 解决GitHub下载速度缓慢的问题
date: 2019-12-03 14:13:10
categories:
tags:
---
众所周知国内访问 GitHub 总会遇到下载速度缓慢、链接意外终止的情况。

为了更加愉快地使用**全球最大同性交友网站**上的优质资源，我们来做一些简单的本机上的调整。

## 第一步，打开本机上的 Hosts 文件

首先，什么是 Hosts 文件?

在互联网协议中，host 表示能够同其他机器互相访问的本地计算机。一台本地机有唯一标志代码，同网络掩码一起组成 ip 地址，如果通过点到点协议通过 ISP 访问互联网，那么在连接期间将会拥有唯一的 ip 地址，这段时间内，你的主机就是一个 host。

在这种情况下，host 表示一个网络节点。host是根据TCP/IP for Windows 的标准来工作的，它的作用是包含IP地址和Host name(主机名)的映射关系，是一个映射IP地址和Host name(主机名)的规定，规定要求每段只能包括一个映射关系，IP 地址要放在每段的最前面，空格后再写上映射的Host name主机名 。对于这段的映射说明用“#”分割后用文字说明。
终端内输入:

```shell
sudo vim /etc/hosts
```

打开之后，我们就要向里面追加信息了。

## 第二步，追加域名的IP 地址

我们可以利用 https://www.ipaddress.com/ 来获得以下两个GitHub域名的IP地址:

* github.com

* github.global.ssl.fastly.net

打开网页后，利用输入框内分别查询两个域名:

先试一下 `github.com`

在标注的IP地址中，任选一个记录下来。

再来是 `github.global.ssl.fastly.net` :

将以上两段IP写入Hosts文件中:

保存。

## 第三步，刷新 DNS 缓存

在终端或 `CMD` 中，执行以下命令:

```shell
ipconfig /flushdns
```

现在再来试一下 git clone 命令，是不是可以轻松过百K了? :)