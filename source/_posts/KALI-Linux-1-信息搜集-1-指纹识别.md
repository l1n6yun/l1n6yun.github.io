title: KALI Linux 1.信息搜集 1(指纹识别)
author: l1n6yun
tags: 
 - 信息搜集
 - curl命令
 - nmap
 - 操作系统识别
 - 渗透测试
categories:
  - 渗透
date: 2019-12-26 19:19:00
---
## 为什么要做信息搜集

> 在渗透测试中，就好比和人打架，你不知道对方的身高、体型、力气有多大。所以再打架前就要通过一些手段，来收集到对方的信息，搜集到的越多越好。

## 使用curl命令

通过 `curl` 命令添加 `--head` 参数来获取相应头，从响应头来判断操作系统

```shell
┌──(kali㉿kali)-[~/Tools/w3af]
└─$ curl --head http://192.168.0.102 
HTTP/1.1 200 OK
Content-Length: 1193
Content-Type: text/html
Content-Location: http://192.168.0.102/iisstart.htm
Last-Modified: Fri, 21 Feb 2003 12:15:52 GMT
Accept-Ranges: bytes
ETag: "0ce1f9a2d9c21:242"
Server: Microsoft-IIS/6.0
MicrosoftOfficeWebServer: 5.0_Pub
X-Powered-By: ASP.NET
Date: Mon, 03 Oct 2022 14:29:35 GMT
```

IIS 版本和操作系统对应表 

| IIS Version | Windows Server Version      |
| ----------- | --------------------------- |
| IIS 5.0     | Windows 2000                |
| IIS 5.1     | Windows XP                  |
| IIS 6.0     | Windows 2003                |
| IIS 7.0     | Windows 2008、Windows Vista |
| IIS 7.5     | Windows 2008 R2、Windows 7  |

## 使用 nmap 命令

**查看服务版本**

```shell
┌──(kali㉿kali)-[~]
└─$ nmap 192.168.0.102 -p 80 -A                                                                   
Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-03 09:42 EDT
Nmap scan report for 192.168.0.102 (192.168.0.102)
Host is up (0.00056s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 6.0
|_http-title: \xBD\xA8\xC9\xE8\xD6\xD0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/6.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.39 seconds
```

**-p \<port ranges>**: Only scan specified ports

**-A**: Enable OS detection, version detection, script scanning, and traceroute

**查看操作版本信息**

```shell
──(kali㉿kali)-[~]
└─$ sudo nmap 192.168.0.102 -O
[sudo] password for kali: 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-03 09:42 EDT
Nmap scan report for 192.168.0.102 (192.168.0.102)
Host is up (0.00033s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
1025/tcp open  NFS-or-IIS
1046/tcp open  wfremotertm
MAC Address: 00:0C:29:86:F6:23 (VMware)
Device type: general purpose
Running: Microsoft Windows XP|2003
OS CPE: cpe:/o:microsoft:windows_xp::sp2 cpe:/o:microsoft:windows_server_2003::sp1 cpe:/o:microsoft:windows_server_2003::sp2
OS details: Microsoft Windows XP SP2 or Windows Server 2003 SP1 or SP2
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 2.46 seconds
```

OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively