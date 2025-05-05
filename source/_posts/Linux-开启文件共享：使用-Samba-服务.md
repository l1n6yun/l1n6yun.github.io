title: Linux 开启文件共享：使用 Samba 服务
author: l1n6yun
tags: 
 - Samba
 - Linux
 - 文件共享
categories: []
date: 2024-07-20 22:19:54
---
在 Linux 系统中，Samba 是一个功能强大的服务，它允许 Linux 和 Windows 系统之间进行文件共享。通过 Samba，你可以轻松地在不同操作系统之间共享文件和打印机。本文将详细介绍如何在 Linux 上配置 Samba 服务，以实现文件共享。

## 安装 Samba

首先，你需要确保你的 Linux 系统上安装了 Samba。在基于 Debian 的系统（如 Ubuntu）上，你可以使用以下命令来安装 Samba：

```bash
sudo apt update
sudo apt install samba
```

这将安装 Samba 服务及其依赖项。

## 配置 Samba

安装完成后，你需要配置 Samba 以便它知道你想要共享哪些目录。这涉及到编辑 Samba 的配置文件 `smb.conf`。

2.  使用文本编辑器打开 `smb.conf` 文件。这里我们使用 `vi` 编辑器，但你也可以使用 `nano` 或其他你喜欢的编辑器：

```bash
sudo vi /etc/samba/smb.conf
```

3.  在 `smb.conf` 文件中，你可以定义多个共享部分，每个部分对应一个共享目录。以下是两个示例共享部分的配置：

```ini
[mnt-rw]
  comment = Read-Write Mount Point
  browseable = yes
  path = /mnt
  force user = root
  create mask = 0775
  directory mask = 0775
  guest ok = Yes
  read only = No

[onecloud-share]
  comment = Read-Only OneCloud Share
  browseable = yes
  path = /mnt
  guest ok = Yes
  read only = yes
```

-   `[mnt-rw]` 部分定义了一个名为 `mnt-rw` 的共享，它指向 `/mnt` 目录。这个共享允许读写访问，并且对所有用户（包括匿名用户）开放。
-   `[onecloud-share]` 部分定义了另一个共享，它同样指向 `/mnt` 目录，但这个共享只允许读访问。

### 配置选项解释

-   `browseable = yes`：这个选项使得共享在网络浏览器中可见。
-   `path = /mnt`：指定共享的文件系统路径。
-   `force user = root`：强制所有连接到此共享的用户以 root 用户身份访问。
-   `create mask = 0775` 和 `directory mask = 0775`：设置文件和目录的默认权限。
-   `guest ok = Yes`：允许匿名访问。
-   `read only = No`：对于 `[mnt-rw]` 共享，这个选项允许写入操作；对于 `[onecloud-share]` 共享，这个选项设置为 `yes`，表示只读。

## 启动 Samba 服务

配置完成后，你需要启动 Samba 服务以使更改生效：

```bash
sudo service smbd start
```

这个命令将启动 Samba 守护进程，使你的共享目录在网络上可用。

## 测试共享

在 Windows 系统上，你可以通过网络邻居访问这些共享，或者在文件资源管理器中输入共享的网络路径（例如 `\\<Linux-Server-IP>\mnt-rw`）来访问。