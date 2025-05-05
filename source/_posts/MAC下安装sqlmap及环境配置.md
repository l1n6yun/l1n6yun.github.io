title: MAC下安装sqlmap及环境配置
description: 生命在于折腾，又把博客折腾到Hexo了。给Hexo点赞。
date: 2019-04-22 10:27:14
categories:
tags: 
 - MAC
 - Git
 - Shell命令
 - 开发环境配置
---
# 1、首先上 git 下 `sqlmap`

```
git clone https://github.com/sqlmapproject/sqlmap.git
```

# 2、接着配置环境

将一下代码添加到 `.bash_profile` 文件中

```
alias sqlmap="/Users/y50/sqlmap/sqlmap.py"
```

> PS：Alias是linux中常用的别名命令，这么好的东东在mac中自然不会舍去。当有一些比较复杂的命令需要经常执行的时候，alias对效率的提升立竿见影。

# 3、bash重新载入配置

```
source ~/.bash_profile
```
即可
以后需要使用 `sqlmap` ，就只需要在 `terminal` 中输入 `sqlmap`