title: 获取阿里云盘Token
author: l1n6yun
tags: []
categories: []
date: 2023-12-15 09:08:00
---
自动获取: Chrome登录 [阿里云盘](https://www.aliyundrive.com/drive/) 后，控制台粘贴

```js
JSON.parse(localStorage.token).refresh_token
```


![upload successful](/images/pasted-68.png)