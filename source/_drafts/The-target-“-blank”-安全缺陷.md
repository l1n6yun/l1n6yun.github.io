---
title: The target=“_blank” 安全缺陷

date: 2017-06-08 20:11:18
categories:
tags: [BUG]
---
国外网友近日曝出大部分网站都忽视了的安全漏洞，包括 `Facebook`，`Twitter`，`Google` 等都被检测出带有 `The target="_blank"` 安全缺陷。

你可以点击 [http://tvvocold.coding.me/target_blank_vulnerability/](http://tvvocold.coding.me/target_blank_vulnerability/) 测试这个安全问题。带有 `target="_blank"` 跳转的网页拥有了浏览器 `window.opener` 对象赋予的对原网页的部分权限，这可能会被恶意网站利用。

```html
<script language="javascript">
window.opener.location = 'https://example.com'
</script>
```

漏洞修复方法：为 `target="_blank"` 加上 `rel="noopener noreferrer"` 属性。

预计该“安全漏洞”影响了 99% 的互联网网站和大部分浏览器，Instagram 已修复这个问题，有趣的是谷歌拒绝修复这个问题，谷歌认为“这属于浏览器缺陷，不能由单一的网站进行有意义的缓解”。
