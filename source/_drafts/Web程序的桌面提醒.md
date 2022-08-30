---
title: Web程序的桌面提醒
date: 2017-01-07 11:23:01
categories: 
tags: 

---
做web开发常会面对的一个问题是，浏览器最小化的时候如何才能向用户发送通知。解决办法大概有三种：让用户有事没事儿搂两眼页面；开发一个桌面客户端；Html5的Notification API。

目前没看到谁采用第一种方案；Yammer采用的是第二种，但仅仅为了这么个小功能，搞个客户端，还得让用户安装，不划算。

Html5的Notification API各大浏览器所支持的程度不一样，使用的话需要考虑浏览器的兼容性。有人看到了这一点，折腾出了一个小巧的桌面提醒库：HTML5 Desktop Notifications。支持的浏览器有：IE9+、Safari6、Firefox、Chrome。

值得一提的是，在IE中需要特殊处理一下才能使用桌面通知：打开链接 -> 拖动地址栏中的图表附加到任务栏。

## 使用

1. 引入js脚本：<script type="text/javascript" src="desktop-notify-min.js"></script>
2. 验证浏览器是否支持桌面通知：notify.isSupported；
	true:支持； false:不支持。
3. 验证浏览器是否允许桌面通知：notify.permissionLevel()；
	返回值：notify.PERMISSION_GRANTED ：没有开启，此时可以调用notify.requestPermission(callback)向用户请求桌面通知的权限。
	notify.PERMISSION_DEFAULT：已经开启。
	notify.PERMISSION_DEFAULT：不允许桌面通知。
4. 产生桌面提醒：notify.createNotification()
	此方法的第一个参数传递标题，第二个参数是一个json对象{body:"Body", icon: "alert.ico",tag:"tag"}.
	返回值是一个通知对象，可以调用该对象的close()方法，关闭桌面通知。

## 各个浏览器的显示效果

以下图片来自官方文档

IE9

![IE9](/images/Web程序的桌面提醒/IE9.png) 

chrome

![chrome](/images/Web程序的桌面提醒/chrome.png) 
 
safari

![safari](/images/Web程序的桌面提醒/safari.png) 