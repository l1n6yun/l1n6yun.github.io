title: PhpStorm 配置 PHP CS Fixer
author: l1n6yun
tags:
  - PhpStorm
categories: []
date: 2022-09-28 00:23:00
---
## PHP CS Fixer 介绍

PHP CS Fixer (PHP Coding Standards Fixer)是一款通过编码标准来修复代码的工具。它支持 PSR 编码规范和其他社区驱动（如 Symfony），还可以根据自己（团队）的风格进行自定义配置。

## PHP CS Fixer 安装

安装 PHP CS Fixer 推荐使用 Composer 来进行安装。可以进行全局安装或者直接安装到项目中

```shell
全局安装
composer global require friendsofphp/php-cs-fixer

为项目安装
composer require --dev friendsofphp/php-cs-fixer 
```

## PhpStorm 配置

在 `PHP > Quality Tools` 找到 `PHP CS Fixer` ,点击配置.


![upload successful](/images/pasted-25.png)

在路径中选这个刚刚安装好的 PHP CS Fixer `C:\Users\l1n6yun\AppData\Roaming\Composer\vendor\bin\php-cs-fixer.bat`.

点击 验证 按钮验证是否安装成功.

![upload successful](/images/pasted-26.png)

点击 `PHP CS Fixer inspection` 配置,在检测页面开启,并**根据项目要求**配置检测规则集

![upload successful](/images/pasted-27.png)

## 配置外部工具

**Program**: `C:\Users\l1n6yun\AppData\Roaming\Composer\vendor\bin\php-cs-fixer.bat`

**Arguments**: `fix "$FileDir$\$FileName$" --using-cache=no`

**Working directory**: `$ProjectFileDir$`


![upload successful](/images/pasted-28.png)

添加快捷键


![upload successful](/images/pasted-29.png)