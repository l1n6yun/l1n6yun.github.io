title: PHP获取curl传输进度
author: l1n6yun
tags: 
 - PHP
 - cURL
 - 上传下载
 - 进度回调
 - 会话管理
categories: []
date: 2017-07-21 00:00:00
---
curl上传或者下载，有以下2个选项：

```php
CURLOPT_NOPROGRESS => false,
CURLOPT_PROGRESSFUNCTION => 'callback',
```

- CURLOPT_NOPROGRESS：是否关闭传输进度，默认是true。
- CURLOPT_PROGRESSFUNCTION：回调函数，curl传输过程中，会每隔一段时间自动调用该函数。我测试过，间隔不到1秒，具体不知道。官方的注释是这样：设置一个回调函数，有五个参数，第一个是cURL的资源句柄，第二个是预计要下载的总字节（bytes）数。第三个是目前下载的字节数，第四个是预计传输中总上传字节数，第五个是目前上传的字节数。（注意回调函数的命名空间。如：CURLOPT_PROGRESSFUNCTION => ‘namespace_xxx\callback’）

设置完成后，需要定义回调函数：

```php
function callback($resource, $downloadSize = 0, $downloaded = 0, $uploadSize = 0, $uploaded = 0)
{
    // php5.5之前的参数是不同的，所以要考虑到兼容性
    if (version_compare(PHP_VERSION, '5.5.0') > 0) {
        $info = array(
            'downloadSize' => $downloadSize,
            'downloaded'   => $downloaded,
            'uploadSize'   => $uploadSize,
            'uploaded'     => $uploaded,
        );
    } else {
        $info = array(
            'downloadSize' => 0,
            'downloaded'   => 0,
            'uploadSize'   => $downloaded,
            'uploaded'     => $uploadSize,
        );
    }

    S('file_upload_' . session('user_auth.uid'), $info, 300); // 可以将结果存放到缓存（这里是ThinkPHP例子）
} 
```

重要：
在curl发起请求时，如果开启了 session，会独占 session ，阻塞其他的请求。所以如果框架默认启用了 session ，在 curl 之前可以用session_write_close() 函数关闭 session 阻塞。
参考：http://www.cnblogs.com/skillCoding/archive/2012/04/09/2439296.html

最后：在进行传输时，可以每隔1秒通过ajax来获取缓存信息，从而显示传输进度。

补充：
传送大文件时，php会超时，注意设置 php-fpm.conf 中的 request_terminate_timeout 值，我设了1000（秒）。
还有个 max_children（进程数） ，进程不够用可改大。
在程序中，可以使用 set_time_limit() 临时增加 php 响应时间。
php.ini中还有 max_execution_time 设置，看攻略说是跟 set_time_limit 累加的，如果攻略是对的，那么这个不用管。

原文链接：https://blog.csdn.net/buer2202/article/details/75364939