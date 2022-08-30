---
title: mysql error:#1062 Duplicate entry '***' for key 1问题解决方法
date: 2017-01-07 11:13:23
categories: 
tags: MySQL

---

> 今天公司的一个网站突然提示MySQL Error Duplicate entry '96624' for key 1错误，经过分析这个问题是由于mysql表中的一个id自增长字段导致。

我将id的int类型改成了bigint就可以了，其实再改回来可能也会好了。可能是数据库备份的时候出现了错误。

开发的网站后台系统在测试过程中出现了这个问题：

```
Invalid Query : Duplicate entry ‘127′ for key 1 
SQL is : INSERT INTO `kq_news` (`Title`,`Author`,`Type`,`Content`,`IsDel`,`Adate`,`Range`,`Lang`) values ('捐款活动','yuanying','3′,”,'0′,NOW(),'2′,'cn') 
```

因为是第一次遇到这样的问题，GOOGLE了一下，类似问题N多，解决方法有很多雷同的，无非就是说修复表（repair），MySQL的修复工具myisamchk工具修复。试了一下，仍然没有解决。 

然后查看了一下数据表结构：
```
CREATE TABLE IF NOT EXISTS `kq_news` ( 
	`Id` tinyint(3) NOT NULL auto_increment, 
	`Title` varchar(90) collate latin1_general_ci NOT NULL, 
	`Content` text collate latin1_general_ci NOT NULL, 
	`Adate` date NOT NULL, 
	`IsDel` tinyint(1) NOT NULL default ‘0′, 
	`Hits` int(5) NOT NULL default ‘0′, 
	`Author` varchar(20) collate latin1_general_ci NOT NULL, 
	`Type` tinyint(1) NOT NULL default ‘1′, 
	`Lang` varchar(2) collate latin1_general_ci NOT NULL, 
	`Range` tinyint(1) NOT NULL default ‘1′, 
	PRIMARY KEY (`Id`) 
) ENGINE=MyISAM DEFAULT CHARSET=latin1 COLLATE=latin1_general_ci ; 
```
终于明白，原来是Id这个自增型字段类型搞错了！转换一下数据类型就搞定了！ 

之后打开了MYSQL手册找到了TINYINT和SMALLINT和INT类型的说明： 

TINYINT[(M)] [UNSIGNED] [ZEROFILL] 
一个很小的整数。有符号的范围是-128到127，无符号的范围是0到255 

SMALLINT[(M)] [UNSIGNED] [ZEROFILL] 
一个小整数。有符号的范围是-32768到32767，无符号的范围是0到65535。 

MEDIUMINT[(M)] [UNSIGNED] [ZEROFILL] 
一个中等大小整数。有符号的范围是-8388608到8388607，无符号的范围是0到16777215。 

INT[(M)] [UNSIGNED] [ZEROFILL] 
一个正常大小整数。有符号的范围是-2147483648到2147483647，无符号的范围是0到4294967295。 

INTEGER[(M)] [UNSIGNED] [ZEROFILL] 
这是INT的一个同义词。 

BIGINT[(M)] [UNSIGNED] [ZEROFILL] 
一个大整数。有符号的范围是-9223372036854775808到9223372036854775807，无符号的范围是0到 
18446744073709551615。 

原来如此！ 
那网上其它的Invalid Query : Duplicate entry ‘32767′ for key 1出错的原因也在于此了！