title: wget命令整站下载做网站镜像
tags: 
 - wget
 - Linux 命令行
 - 网络
 - Web Crawling
 - Software Development
date: 2017-07-07 16:57:15
categories:
---
在 linux 下完整的用 wget 命令整站采集网站做镜像的命令是：

```shell
wget -m -e robots=off -U "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6" "http://www.example.com/"
```

wget命令参数注释：

`-e robots=off` #让 wget 耍流氓无视 robots.txt 协议

`-U "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6"`	#伪造 agent 信息
