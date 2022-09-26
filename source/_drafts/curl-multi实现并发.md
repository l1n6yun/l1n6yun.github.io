title: curl_multi实现并发
date: 2019-12-24 09:46:19
categories:
tags:
---
## 普通请求

```php
$result = [];
for ($i = 0; $i < 10; $i++) {
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, "http://www.example.com");
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    $result[] = curl_exec($ch);
    curl_close($ch);
}
```

运行结果

```shell
real    0m22.526s
user    0m0.030s
sys     0m0.010s
```

##  curl_multi 并发


```php
// 创建cURL资源
$chs = [];
for ($i = 0; $i < 10; $i++) {
    $chs[$i] = curl_init();
    curl_setopt($chs[$i], CURLOPT_URL, "http://www.example.com");
    curl_setopt($chs[$i], CURLOPT_RETURNTRANSFER, 1);
}

// 创建批处理cURL句柄
$multi = curl_multi_init();

// 增加句柄
foreach ($chs as $ch) {
    curl_multi_add_handle($multi, $ch);
}

// 执行批处理句柄
$running = null;
do {
    usleep(10000);
    curl_multi_exec($multi, $running);
} while ($running > 0);

// 关闭全部句柄
$result = [];
foreach ($chs as $ch) {
    $result[] = curl_multi_getcontent($ch);
    curl_multi_remove_handle($multi, $ch);
}
curl_multi_close($multi);

//print_r($result);
```

运行结果

```shell
real    0m2.517s
user    0m0.060s
sys     0m0.020s
```

## curl_multi_select

### 说明

```php
curl_multi_select （ resource $multi [， float $timeout= 1.0 ]）： int
```

阻塞直到 `cURL` 批处理连接中有活动连接。成功时返回描述符集合中描述符的数量。失败时，select失败时返回-1，否则返回超时(从底层的select系统调用).

```php
$active = null;
do {
    $mrc = curl_multi_exec($multi, $active);
} while ($mrc == CURLM_CALL_MULTI_PERFORM);

while ($active && $mrc == CURLM_OK) {
    // 等待任何curl连接上的活动
    if (curl_multi_select($multi) == -1) {
        usleep(100);
    }

    // 继续执行直到curl准备好为我们提供更多的数据
    do {
        $mrc = curl_multi_exec($multi, $active);
    } while ($mrc == CURLM_CALL_MULTI_PERFORM);
}
```

运行结果

```shell
real    0m2.470s
user    0m0.030s
sys     0m0.010s
```

 这样执行的好处是`$multi`批处理中的 `$ch` 句柄会在读取或写入数据结束后(`$mrc == CURLM_OK`),进入 `curl_multi_select($multi)` 的阻塞阶段，而不会在整个 `$multi` 批处理执行时不停地执行 `curl_multi_exec` ,白白浪费CPU资源.