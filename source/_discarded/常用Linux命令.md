title: '常用Linux命令 '
tags:
  - linux
date: 2017-06-25 21:48:55
categories:
---
# 磁盘命令

* 查看磁盘和分区分布

```sh
# lsblk
NAME MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sdb    8:16   0  512M  0 disk [SWAP]
sdc    8:32   0   10G  0 disk
sda    8:0    0  9.5G  0 disk /
```

# 网络命令

* 显示所有活动的网络连接

```sh
# netstat -na
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:6003            0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 119.102.112.204:6003    60.18.18.138:24261    ESTABLISHED
udp        0      0 0.0.0.0:6003            0.0.0.0:*
udp        0      0 0.0.0.0:49056           0.0.0.0:*
udp        0      0 0.0.0.0:33757           0.0.0.0:*
udp        0      0 0.0.0.0:68              0.0.0.0:*
udp        0      0 127.0.0.1:323           0.0.0.0:*
udp6       0      0 :::18267                :::*
udp6       0      0 ::1:323                 :::*
raw6       0      0 :::58                   :::*                    7
Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  2      [ ]         DGRAM                    8472     /run/systemd/notify
unix  2      [ ACC ]     STREAM     LISTENING     8476     /run/systemd/private
````

* 显示处于监听状态的的应用程序、进程号、端口号

```sh
# netstat -aultnp
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:6003            0.0.0.0:*               LISTEN      3512/python2
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      3510/sshd
udp        0      0 0.0.0.0:6003            0.0.0.0:*                           3512/python2
udp        0      0 0.0.0.0:49056           0.0.0.0:*                           3512/python2
udp        0      0 0.0.0.0:33757           0.0.0.0:*                           3291/dhclient
udp        0      0 0.0.0.0:68              0.0.0.0:*                           3291/dhclient
udp        0      0 127.0.0.1:323           0.0.0.0:*                           3212/chronyd
udp6       0      0 :::18267                :::*                                3291/dhclient
udp6       0      0 ::1:323                 :::*                                3212/chronyd
```
