title: 如何搭建自己的 composer 包
author: l1n6yun
date: 2022-03-14 20:14:38
tags:
---
> composer 是 PHP 的依赖管理工具，本篇文章就来说明如何构建一个包，并提交到 Packagist ，这样别人就可以方便地通过 composer 使用你的包了。

## 初始化

```shell
# composer init


  Welcome to the Composer config generator



This command will guide you through creating your composer.json config.

Package name (<vendor>/<name>) [l1n6yun/hello]: l1n6yun/hello
Description []: This is a demo package
Author [l1n6yun <l1n6yun@gmail.com>, n to skip]:
Minimum Stability []: dev
Package Type (e.g. library, project, metapackage, composer-plugin) []: library
License []: MIT

Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? no
Would you like to define your dev dependencies (require-dev) interactively [yes]? no
Add PSR-4 autoload mapping? Maps namespace "L1n6yun\Hello" to the entered relative path. [src/, n to skip]:

{
    "name": "l1n6yun/hello",
    "description": "This is a demo package",
    "type": "library",
    "license": "MIT",
    "autoload": {
        "psr-4": {
            "L1n6yun\\Hello\\": "src/"
        }
    },
    "authors": [
        {
            "name": "l1n6yun",
            "email": "l1n6yun@gmail.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {}
}

Do you confirm generation [yes]?
Generating autoload files
Generated autoload files
PSR-4 autoloading configured. Use "namespace L1n6yun\Hello;" in src/
Include the Composer autoloader with: require 'vendor/autoload.php';
```

### Package name

包的名称。它是由供应商名称和项目名称组成，由 `/` 隔开。

### Description

包的简短描述。用来告诉使用者这个包是干什么用的。

### Author

包的作者名。

### Minimum Stability

最小稳定版本。越往下，稳定性越高，BUG越少。

- dev
- alpha
- beta
- RC（补丁）
- stable

### Package Type

包的类型。用于自定义安装逻辑。

-  **library**：这个是默认设置。它会将文件复制到 `vendor` 目录
- **project**：项目。注意这里的项目不是库。
- **metapackage**：一个包含需求的空包，会触发  他们的安装，但不包含文件，不会向  文件系统。 因此，它不需要 dist 或 source 键  可安装。 
- **composer-plugin**：类型的包 `composer-plugin`可以提供一个  具有自定义类型的其他软件包的安装程序。

### License

包的许可证。  这可以是字符串或字符串数组。 

最常见许可证

- Apache-2.0
- BSD-2-Clause
- BSD-3-Clause
- BSD-4-Clause
- GPL-2.0-only / GPL-2.0-or-later
- GPL-3.0-only / GPL-3.0-or-later
- LGPL-2.1-only / LGPL-2.1-or-later
- LGPL-3.0-only / LGPL-3.0-or-later
- MIT

> 闭源项目可以使用 `proprietary`

## 撸码

在项目的 `src` 目录下创建 `Tools` 类

```php
<?php

namespace L1n6yun\Hello;

class Tools
{
    public static function hello()
    {
        echo 'Hello, World!';
    }
}
```

测试一下

```php
# php -a
Interactive shell

php > require 'vendor/autoload.php';
php > L1n6yun\Hello\Tools::hello();
Hello, World!
```

## 发布

### 上传代码

添加 `.gitignore` 文件，来忽略不需要的文件

```
.idea/
/vendor/
composer.lock
```

自行上传到自己的 GitHub 上

### 提交包

Packagist 上的 [提交页面](https://packagist.org/packages/submit) 填写自己的仓库地址，点击 `Check` ，然后点击 `Submit` 不出意外的话就发布成功了。

![upload successful](/images/pasted-7.png)
