title: PHPStorm 控制台乱码
author: l1n6yun
tags: 
 - PHPStorm
 - 控制台乱码
 - 字符编码
categories: []
date: 2024-07-06 13:29:00
---
> PHPStorm 控制台出现乱码的情况，往往是由于控制台所采用的字符编码和输出内容的编码存在差异所致。比如说，控制台或许默认运用的是 GBK 编码，然而您的 PHP 代码输出的却是 UTF-8 编码的内容，抑或是其他的编码形式。


![upload successful](/images/pasted-69.png)

开启 PHPStorm 的设置页面，在其中进行搜索“Console Encoding”，接着选中“Default Encoding”这一选项，并将“UTF-8”设定为默认编码。

![upload successful](/images/pasted-70.png)

再次执行相关命令，此时控制台理应能够正常显示内容。

![upload successful](/images/pasted-71.png)