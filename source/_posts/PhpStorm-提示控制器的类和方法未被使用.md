title: PhpStorm 提示控制器的类和方法未被使用
author: l1n6yun
tags: []
categories: []
date: 2022-09-26 22:35:00
---
在 `Editor > Inspections` 中找到 `Unused declaration` ，在 `Options` 中的 `Entry points` 表中中点击 `Code patterns`

![upload successful](/images/pasted-23.png)

根据项目的控制器命名空间添加2条记录。(Member为空，表示构造函数，*表示所有函数。支持正则匹配，可以根据实际代码调整。)

![upload successful](/images/pasted-24.png)