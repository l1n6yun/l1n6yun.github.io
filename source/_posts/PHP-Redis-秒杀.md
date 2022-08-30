title: PHP Redis 秒杀
categories: []
tags: []
date: 2018-11-23 09:39:00
---

```php
/**
 * Redis 初始化
 * 
 * @param int $goodsId 商品ID
 * @param int $number 商品数量
 */
function initRedis($goodsId, $number)
{
    $storeKey = "goods_{$goodsId}_store";

    $redis = new redis();
    $redis->connect('127.0.0.1', 6379);

    $redis->del($storeKey);

    for ($i = 0; $i < $number; $i++) {
        $redis->lpush($storeKey, 1);
    }
}
```

```php
/**
 * 秒杀入口
 * 
 * @param int $goodsId 商品ID
 * @return bool
 */
function kills($goodsId)
{
    $storeKey = "goods_{$goodsId}_store";

    $redis = new redis();
    $redis->connect('127.0.0.1', 6379);

    $count = $redis->lpop($storeKey);
    if ($count) {
        $data = "";
        $data["order_sn"] = rand(1000, 9999);	// 测试订单号

        // 1. 直接进行数据库操作
        $orderModel = new orderModel();
        $result = $orderModel->save($data);

        if ($result !== false) {
            return true;
        } else {
            $redis->lpush($storeKey, 1);
            return false;
        }

        // 2. 添加队列后继操作
        Queue::push($job, $data, 'seckill');
    }
}
```

