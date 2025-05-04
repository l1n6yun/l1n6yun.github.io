title: CURL证书
description: 'curl: (60) SSL certificate : unable to get local issuer certificate'
tags: []
categories: []
date: 2018-07-05 16:19:00
---
出现错误

```
curl: (60) SSL certificate : unable to get local issuer certificate
```

错误原因 ：未正确配置 CA 证书

[证书下载](/file/cacert.pem)

下载后将证书位置保存在 `php.ini`

`curl.cainfo = "d:/wamp/php/cacert.pem"`
