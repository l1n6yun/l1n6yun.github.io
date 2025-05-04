title: ARP欺骗工具arpspoof的用法
author: l1n6yun
tags: 
 - ARP欺骗
 - 网络安全
 - arpspoof
 - 网络攻击
categories: []
date: 2021-09-11 16:52:00
---
### ARP工具

arpspoof 是一款进行arp欺骗的工具，攻击者可以通过它来毒化受害者arp缓存，将网关mac替换为攻击者mac，然后攻击者可截获受害者发送和收到的数据包，可获取受害者账户、密码等相关敏感信息。ARP欺骗，是让目标主机的流量经过主机的网卡，再从网关出去，而网关也会把原本流入目标机的流量经过我得电脑。

```
Usage: arpspoof [-i interface] [-c own|host|both] [-t target] [-r] host
```

#### ARP断网攻击

在实验之前我们首先要知道ARP断网攻击是局域网攻击，我们要保证目标主机必须和自己处于一个局域网内，且自己和目标主机网络应该是通的。

首先可以使用 nmap 扫描局域网中存活的地址,探测到的存活地址，就是我们的目标主机,然后使用 arpspoof 实施 ARP 欺骗。

![upload successful](/images/pasted-63.png)

在使用arp欺骗前先开启Kali的IP转发

![upload successful](/images/pasted-64.png)

链接目标机查看一下网络状态

![upload successful](/images/pasted-65.png)

使用 ettercap 对数据进行抓取

![upload successful](/images/pasted-66.png)


![upload successful](/images/pasted-67.png)