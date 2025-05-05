title: 设置获取cookie，setCookie，getCookie
categories: []
date: 2018-03-15 21:34:14
description:
tags: 
 - JavaScript
 - 前端开发
 - Web开发
---

## 设置cookie

```javascript
function setCookie(name,value) 
{ 
    var Days = 30; 
    var exp = new Date(); 
    exp.setTime(exp.getTime() + Days*24*60*60*1000); 
    document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString(); 
}
```

## 读取cookie

```javascript
function getCookie(name) 
{ 
    var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)"); 
　　 return (arr=document.cookie.match(reg))?unescape(arr[2]):null;
}
```

## 删除cookie（将cookie设置过期即可）

```javascript
function delCookie(name) 
{ 
    var exp = new Date(); 
    exp.setTime(exp.getTime() - 1); 
    var cval=getCookie(name); 
    if(cval!=null) 
        document.cookie= name + "="+cval+";expires="+exp.toGMTString(); 
}
```

> escape(string) 函数可对字符串进行编码，这样就可以在所有的计算机上读取该字符串。 
> unescape(string) 函数可对通过 escape() 编码的字符串进行解码。