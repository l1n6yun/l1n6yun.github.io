title: PHP 接受命令行参数的三种方式
description: 通常 PHP 都作为 HTTP 方式来请求，有些时候需要在 shell 命令写把 PHP 作为脚本来执行，比如定时任务。
categories: []
tags: []
date: 2018-12-29 23:02:00
---
> 通常 PHP 都作为 HTTP 方式来请求，有些时候需要在 shell 命令写把 PHP 作为脚本来执行，比如定时任务。这样和涉及到在 shell 命令下如何给 PHP 传参的问题，通常有三种方式传参。

# 使用 $argc 和 $argv 变量接受

```php
/**
 * 使用 $argc 和 $argv 接受参数
 */

print "接受到了 {$argc} 个参数\r\n";
print_r($argv);
```

执行结果如下

```shell
php test.php

接受到了 1 个参数
Array
(
    [0] => test.php
)

php test.php a b c d
接受到了 5 个参数
Array
(
    [0] => test.php
    [1] => a
    [2] => b
    [3] => c
    [4] => d
)
```

# 使用 getopt 函数

```php
/**
 * 使用 getopt 函数
 */

$paramArray = getopt('a:b:');
print_r($paramArray);
```

执行结果如下

```shell
php test.php -a 1 -b 2
Array
(
    [a] => 1
    [b] => 2
)

```

# 提示用户输入

```php
/**
 * 提示用户输入，类似Python
 */

fwrite(STDOUT,'Please enter your name: ');
print 'The name you entered is: '.fgets(STDIN);
```

执行结果如下

```shell
php test.php
Please enter your name: helen
The name you entered is: helen
```

你也可以这么干，不让用户输入空内容

```php
$fs = true;

do {
    if ($fs) {
        fwrite(STDOUT, 'Please enter your name:');
        $fs = false;
    } else {
        fwrite(STDOUT, 'Sorry, the name cannot be empty, please re-enter your name:');
    }

    $name = trim(fgets(STDIN));

} while (!isset($name));

print "The name you entered: {$name}\r\n";
```