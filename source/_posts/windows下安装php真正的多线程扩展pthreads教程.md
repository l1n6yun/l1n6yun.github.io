title: windows下安装php真正的多线程扩展pthreads教程
tags:
  - php
date: 2017-10-16 17:44:51
categories:
---
扩展地址：http://docs.php.net/manual/zh/book.pthreads.php

## 注意事项

php5.3 或以上，且为线程安全版本。apache 和 php 使用的编译器必须一致。

通过 `phpinfo()` 查看 `Thread Safety` 为 `enabled` 则为线程安全版。

通过 `phpinfo()` 查看 `Compiler` 项可以知道使用的编译器。本人的为：MSVC9 (Visual C++ 2008)。

## 本人使用环境

32位 windows xp sp3 ，wampserver2.2d（php5.3.10-vc9 + apache2.2.21-vc9）。

## 一、下载 pthreads 扩展

下载地址：http://windows.php.net/downloads/pecl/releases/pthreads

根据本人环境，我下载的是 pthreads-2.0.8-5.3-ts-vc9-x86 。

	2.0.8 代表 pthreads 的版本。
	5.3 代表 php 的版本。
	ts 表示 php 要线程安全版本的。
	vc9 表示 php 要 Visual C++ 2008 编译器编译的。
	x86 则表示32位的


## 二、安装 pthreads 扩展

复制 `php_pthreads.dll` 到目录 `bin\php\ext\` 下面。（本人路径D:\wamp\bin\php\php5.3.10\ext）

复制 `pthreadVC2.dll` 到目录 `bin\php\` 下面。（本人路径D:\wamp\bin\php\php5.3.10）

复制 `pthreadVC2.dll` 到目录 `C:\windows\system32` 下面。

打开 php 配置文件 `php.ini`。在后面加上 `extension=php_pthreads.dll`

提示！Windows系统需要将 `pthreadVC2.dll` 所在路径加入到 `PATH` 环境变量中。

我的电脑--->鼠标右键--->属性--->高级--->环境变量--->系统变量--->找到名称为 Path 的--->编辑--->在变量值最后面加上 pthreadVC2.dll 的完整路径（本人的为C:\WINDOWS\system32\pthreadVC2.dll）。

## 三、测试 pthreads 扩展

```php
class AsyncOperation extends \Thread {
    public function __construct($arg){
        $this->arg = $arg;
    }
    public function run(){
        if($this->arg){
            printf("Hello %s\n", $this->arg);
        }
    }
}
$thread = new AsyncOperation("World");
if($thread->start())
    $thread->join();
```

运行以上代码出现 Hello World ，说明 pthreads 扩展安装成功！ 