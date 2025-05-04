title: windows7登陆界面背景修改
tags: 
 - windows7
 - 注册表
 - 登录界面
 - 界面优化
categories: []
date: 2016-11-25 10:53:00
---
### 执行reg

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\LogonUI\Background]
"OEMBackground"=dword:00000001
```
### 添加图片

打开文件夹`C:\Windows\System32\oobe\info\backgrounds`(如果没有这个文件夹的请自行创建)。

添加自己选择好的图片，文件名修改成`backgroundDefault.jpg`.

**注：我们编辑好的登录界面的背景图片`backgroundDefault.jpg`，其体积一定控制在250KB以内；否则，我们修改后的登录界面的背景图片就无法了.**