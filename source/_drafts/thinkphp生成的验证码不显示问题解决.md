---
title: thinkphp生成的验证码不显示问题解决
date: 2016-12-15 11:48:29
categories: 
tags: thinkphp

---
在调用验证码之前加上 `ob_clean();`

不显示验证码的代码：

```php
public function verify(){
	$verify = new \Think\Verify();
	$verify->entry();
}
```

修改为：

```php
public function verify(){

	ob_clean();

	$verify = new \Think\Verify();
	$verify->entry();
}
```
这样的话，保存再刷新一次，验证码就出现了

### 分析：
1. `ob_clean` 这个函数的作用：
用来丢弃输出缓冲区中的内容，如果你的网站有许多生成的图片类文件，那么想要访问正确，就要经常清除缓冲区
2. 在出现问题的页面查看源代码，发现在页面尾部出现了一堆其他代码（原因不明）