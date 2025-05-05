title: file cache class of php
date: 2020-01-08 18:00:31
categories:
tags: 
 - PHP
 - 文件缓存
---

```php
/**
 * Class Cache
 */
class Cache
{
    /**
     * @var string 缓存目录
     */
    public static $cache_path = "";

    /**
     * 设置缓存
     * @param string $name 缓存变量名
     * @param mixed $value 存储数据
     * @param int $expire 有效时间（秒）
     * @return string
     */
    public static function setCache($name, $value, $expire = null)
    {
        if (is_null($expire)) {
            $expire = 0;
        }
        $file = self::_getCacheName($name);
        $data = serialize($value);
        $data = "<?php\n//" . sprintf('%012d', $expire) . "\n exit();?>\n" . $data;
        file_put_contents($file, $data);
        return $file;
    }

    /**
     * 获取缓存
     * @param string $name 缓存变量名
     * @param mixed $default 默认值
     * @return mixed
     */
    public static function getCache($name,$default = false)
    {
        $file = self::_getCacheName($name);
        if (file_exists($file) && ($content = file_get_contents($file))) {
            $expire = (int)substr($content, 8, 12);
            if ($expire === 0 || filemtime($file) + $expire >= time()) {
                $content = unserialize(substr($content, 32));
                return $content;
            }
            self::delCache($name);
        }
        return $default;
    }

    /**
     * 清楚缓存
     * @param $name
     * @return bool
     */
    public static function delCache($name)
    {
        $file = self::_getCacheName($name);
        return file_exists($file) ? unlink($file) : true;
    }

    /**
     * 应用缓存目录
     * @param string $name
     * @return string
     */
    private static function _getCacheName($name)
    {
        if (empty(self::$cache_path)) {
            self::$cache_path = dirname(dirname(__DIR__)) . DIRECTORY_SEPARATOR . DIRECTORY_SEPARATOR;
        }
        self::$cache_path = rtrim(self::$cache_path, '/\\') . DIRECTORY_SEPARATOR;
        if (!file_exists(self::$cache_path)) {
            mkdir(self::$cache_path, 0755, true);
        }
        return self::$cache_path . $name;
    }
}
```

调用方法

```php
// 设置目录
Cache::$cache_path = "./cache";

// 设置缓存
Cache::setCache('key', 'value','7200');

// 获取缓存
$result = Cache::getCache('key');
print_r($result);

Cache::delCache('aaa');
```