title: windows mysql 自动备份的几种方法
tags:
  - MySQL
date: 2017-02-07 15:13:29
categories:
---
> 基于之前的文章方法，加入批处理命令即可实现自动备份。只是由于批处理命令中对于备份文件的名字按照时间命名比较特别，所以特别整理一文。

### 复制date文件夹备份

假想环境：
MySQL   安装位置：C:\MySQL
论坛数据库名称为：bbs
数据库备份目的地：C:\db_bak\

新建db_bak.bat，写入以下代码

```Bat
net stop mysql
xcopy c:\mysql\data\bbs\*.* c:\db_bak\bbs\%date:~0,10%\ /S /I
net start mysql
```

然后使用Windows的“计划任务”定时执行该批处理脚本即可。（例如：每天凌晨3点执行back_db.bat）

解释：备份和恢复的操作都比较简单，完整性比较高，控制备份周期比较灵活，例如，用%date:~0,10%。此方法适合有独立主机但对mysql没有管理经验的用户。缺点是占用空间比较多，备份期间mysql会短时间断开（例如：针对30M左右的数据库耗时5s左右）,针对%date:~0,10%的用法参考           。

### mysqldump备份成sql文件

假想环境：

MySQL   安装位置：C:\MySQL
论坛数据库名称为：bbs
MySQL root   密码：123456
数据库备份目的地：D:\db_backup\

脚本：

```Bat
@echo off
set "Ymd=%date:~,4%%date:~5,2%%date:~8,2%"
C:\MySQL\bin\mysqldump --opt -u root --password=123456 bbs > D:\db_backup\bbs_%Ymd%.sql
@echo on
```

将以上代码保存为backup_db.bat

然后使用Windows的“计划任务”定时执行该脚本即可。（例如：每天凌晨5点执行back_db.bat）

说明：此方法可以不用关闭数据库，并且可以按每一天的时间来名称备份文件。

通过%date:~5,2%来组合得出当前日期，组合的效果为yyyymmdd,date命令得到的日期格式默认为yyyy-mm-dd(**如果不是此格式可以通过pause命令来暂停命令行窗口看通过%date:~,20%得到的当前计算机日期格式**)，所以通过%date:~5,2%即可得到日期中的第五个字符开始的两个字符，例如今天为2009-02-05,通过%date:~5,2%则可以得到02。（日期的字符串的下标是从0开始的）

### 利用WinRAR对MySQL数据库进行定时备份。 

对于MySQL的备份，最好的方法就是直接备份MySQL数据库的Data目录。下面提供了一个利用WinRAR来对Data目录进行定时备份的方法。

首先当然要把WinRAR安装到计算机上。

将下面的命令写入到一个文本文件里

```Bat
net stop mysql
c:\progra~1\winrar\winrar a -ag -k -r -s d:\mysql.rar d:\mysql\data
net start mysql
```

保存，然后将文本文件的扩展名修改成CMD。进入控制面版，打开计划任务，双击“添加计划任务”。在计划任务向导中找到刚才的CMD文件，接着为这个任务指定一个运行时间和运行时使用的账号密码就可以了。

这种方法缺点是占用时间比较多，备份期间压缩需要时间，mysql断开比第一种方法更多的时间，但是对于文件命名很好。