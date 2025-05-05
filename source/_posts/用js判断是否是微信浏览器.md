title: 用js判断是否是微信浏览器
description: 生命在于折腾，又把博客折腾到Hexo了。给Hexo点赞。
categories: []
tags: 
 - JavaScript
 - 前端开发
 - 客户端
 - 浏览器检测
date: 2017-04-24 10:47:00
---

```javascript
//判断是否是微信浏览器的函数
function isWeiXin(){
    //window.navigator.userAgent属性包含了浏览器类型、版本、操作系统类型、浏览器引擎类型等信息，这个属性可以用来判断浏览器类型
    var ua = window.navigator.userAgent.toLowerCase();
    //通过正则表达式匹配ua中是否含有MicroMessenger字符串
    if(ua.match(/MicroMessenger/i) == 'micromessenger'){
        return true;
    }else{
        return false;
    }
}
```