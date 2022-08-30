---
title: Maximum execution time of 30 seconds exceeded解决办法

date: 2017-06-22 13:36:01
categories:
tags: [php]
---
今天给朋友配置wamp的时候，刚刚搭建好，打开一个本地站就出现这个错误，

`Maximum execution time of 30 seconds exceeded`，今天把这个错误的解决方案总结一下：

简单总结一下解决办法：

### 报错一：内存超限。

利用循环分批导入；

每个循环内部开始处使用 `sleep(5);` 语句，做延迟执行，防止服务器内存同一时间占用过多，里面数字据情况修改；

每个循环内部结束地方使用 `ob_flush();`刷新输出缓冲

`flush();` 将当前为止程序的所有输出发送到用户的浏览器

两者必须同时使用来刷新输出缓冲

### 报错二：30秒运行超时的错误（Maximum execution time of 30 seconds exceeded）

解决办法：

#### 方法一，修改 `php.ini` 文件

```ini
max_execution_time = 30; Maximum execution time of each script, in seconds
```

把它设置成需要的值就可以了。如果设置成0的话，就是永不过期。

#### 方法二，修改php执行文件

```php
<?php
set_time_limit(0);
?>
```

把它设置成需要的值就可以了。如果设置成0的话，就是永不过期。
