---
title: Apache优化：修改最大并发连接数
description: 生命在于折腾，又把博客折腾到Hexo了。给Hexo点赞。
date: 2017-12-19 16:24:27
categories:
tags:
---
MPM(多路处理模块)

常见:

1. perfork 预处理进程方式
2. worker 工作者模式
3. winnt 在windows使用

案例:把apache的最大并发数配置成1000个

## 首先确认apache的mpm方式

```
cmd>httpd.exe -l #可以看到是什么模式了
```

这里就看 `mpm_xxx.c` 这个 `xxx` 就是那个了

## 修改httpd.conf文件

搜索 `mpm` ,找到 `Server-pool management(MPM specific)`

去掉 `# Include conf/extra/httpd-mpm.conf`

## 修改 `conf/extra/httpd-mpm.conf` 文件

### prefork模式就修改这里

```
<IfModule mpm_prefork_module>
	StartServers 5 # 预先开启的进程
	MinSpareServers 5 # 最小预留5个
	MaxSpareServers 10 # 最大留10
	MaxClients 150 # 最多并发多少个 *
	MaxRequestsPerChild 0 # 最多请求多少次 0不限制
</IfModule>
```

### winnt模式

```
<IfModule mpm_winnt_module>
	ThreadsPerChild 150 # 最大并发数 *
	MaxRequestsPerChild 0 # 最多处理多少次请求 0不限制
</IfModule>
```

修改后面有*的那个字段的数值然后重新启动 `apache`

说明:配置到多大,不一定就可能支撑这么大的并发,考虑到本身 `apache` 所在的机器硬件性能(如:内存,CPU,硬盘IO)

系统是 `linux/unix` ,配置 `perfork`

```
<IfModule mpm_prefork_module>
	StartServers 5
	MinSpareServers 5
	MaxSpareServers 10
	MaxClients 150 *#并发量
	MaxRequestsPerChild 0
</IfModule>
```

给大家一个合理的建议配置,对大部份网站,中型网站配置

```
<IfModule mpm_prefork_module>
	StartServers 5 # 预先启动
	MinSpareServers 5
	MaxSpareServers 10 # 最大空闲进程
	ServerLimit 1500 *# 用于修改apache编程参数
	MaxClients 1000 *# 最大并发数
	MaxRequestsPerChild 0
</IfModule>
```

注:apache2.2以后才有的ServerLimit这个参数,其中ServerLimit数值大于MaxClients数值

如果网站的pv值百万

```
ServerLimit 2500 *# 用于修改apache编程参数
MaxClients 2000 *# 最大并发数
```

注:调到这就是极限了,要是网站访问还是大,哪就要增加apache服务器了