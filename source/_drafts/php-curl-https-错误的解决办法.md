---
title: php curl https 错误的解决办法
date: 2017-10-16 17:40:30
categories:
tags: [php]
---

报错信息为

```
cURL error 60: SSL certificate problem: self signed certificate in certificate chain (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)
```

### 解决办法

在php.ini 找到 `curl.cainfo` 修改为

```
curl.cainfo = "d:/wamp/bin/php/php5.5.12/cacert.pem"
```

[证书下载](http://curl.haxx.se/ca/cacert.pem)