title: php-cs-fixer
author: l1n6yun
tags: 
 - PHP
 - Composer
 - 代码修复
categories: []
date: 2022-09-28 23:52:41
---
## 介绍

PHP CS Fixer (PHP Coding Standards Fixer)是一款通过编码标准来修复代码的工具。它支持 PSR 编码规范和其他社区驱动（如 Symfony），还可以根据自己（团队）的风格进行自定义配置。

## 安装

安装 PHP CS Fixer 推荐使用 Composer 来进行安装。可以进行全局安装或者直接安装到项目中

```shell
# 全局安装
$ composer global require friendsofphp/php-cs-fixer

# 为项目安装
$ composer require --dev friendsofphp/php-cs-fixer 
```

## 用法

### describe 查看规则规则集


![upload successful](/images/pasted-30.png)


### fix 修复一个文件或者目录

使用 `fix` 命令可以对文件或者目录进行修复

```shell
$ php-cs-fixer fix /path/to/dir
$ php-cs-fixer fix /path/to/file
```

`--path-mode` 输出格式选项，支持 txt、json、xml、checkstyle、junit 和 gitlab。（默认txt）

`--quiet` 不输出任何信息。

`-v --verbose` 选项将显示应用的规则。 
`-vv` 啰嗦
`-vvv` 调试

![upload successful](/images/pasted-32.png)

出现“修复后 linting 期间报告的错误”，可以使用它来更详细地进行调试

![upload successful](/images/pasted-31.png)


`--rules` 指定修复规则

```shell
$ php-cs-fixer fix ./ --rules=@PSR12

$ php-cs-fixer fix ./ --rules=line_ending,full_opening_tag,indentation_type

$ php-cs-fixer fix ./ --rules=-full_opening_tag,-indentation_type

$ php-cs-fixer fix ./ --rules=@Symfony,-@PSR1,-blank_line_before_statement,strict_comparison

$ php-cs-fixer fix ./ --rules='{"concat_space": {"spacing": "none"}}'
```

`--dry-run` 运行修复程序但不修改文件

`--diff` 以 udiff 格式输出修改内容

`--allow-risky` 是否运行有风险的修改，传入参数（yes or no）

`--stop-on-violation` 修复一个文件后停止执行

`--show-progress` 显示处理进度

### 退出代码

退出代码 fix命令是使用以下位标志构建的：

- 0 - 好的。
- 1 - 一般错误（或 PHP 最低要求不匹配）。
- 4 - 某些文件的语法无效（仅在试运行模式下）。
- 8 - 某些文件需要修复（仅在试运行模式下）。
- 16 - 应用程序的配置错误。
- 32 - Fixer 的配置错误。
- 64 - 应用程序中出现异常。