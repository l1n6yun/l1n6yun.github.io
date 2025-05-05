title: win10下右键菜单添加“打开cmd”
description: >-
  最近使用cmd比较多，就想在某个文件夹下右键打开cmd，这样不用每次都在默认情况下切换目录。无奈win10
  1703版本下shift+右键不能打开cmd，只能打开powershell。
categories: []
tags: 
 - Windows 10
 - Registry编辑
 - 右键菜单
 - CMD
date: 2019-08-08 12:31:00
---
> 最近使用cmd比较多，就想在某个文件夹下右键打开cmd，这样不用每次都在默认情况下切换目录。无奈win10 1703版本下shift+右键不能打开cmd，只能打开powershell。

好，那就自己整一个吧。

首先，在桌面新建一个文本文档。

```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\cmd_here]
@="在此处打开命令行"
"Icon"="cmd.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\cmd_here\command]
@="\"C:\\Windows\\System32\\cmd.exe\""

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\cmdPrompt]
@="在此处打开命令行"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\cmdPrompt\command]
@="\"C:\\Windows\\System32\\cmd.exe\" \"cd %1\""

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\cmd_here]
@="在此处打开命令行"
"Icon"="cmd.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\cmd_here\command]
@="\"C:\\Windows\\System32\\cmd.exe\""
```

然后、将上面内容粘贴到该文本文档中，保存。**并将该文本文档以.reg结尾即可，名字可以随意取。**（PS：`@="此处打开命令行"`  该引号内文字可以随意修改成你想要显示的文字）

最后，双击注册一下就可以了，结果右键菜单中就有了。