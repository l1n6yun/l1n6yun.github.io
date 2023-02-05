title: ThinkPHP连接SQLite
tags:
  - ThinkPHP
  - SQLite
date: 2017-04-14 21:14:34
---
配置：

```php
'DB_TYPE' => 'sqlite',
'DB_NAME' => DATA_PATH.'/test.db',
```

文件：\Library\Think\Db\Driver\Sqlite.class.php

```php
namespace Think\Db\Driver;

use Think\Db\Driver;

/**
 * Sqlite数据库驱动
 */
class Sqlite extends Driver
{

    /**
     * 解析pdo连接的dsn信息
     * @access public
     * @param array $config 连接信息
     * @return string
     */
    protected function parseDsn($config)
    {
        $dsn = 'sqlite:' . $config['database'];
        return $dsn;
    }

    /**
     * 取得数据表的字段信息
     * @access public
     * @return array
     */
    public function getFields($tableName)
    {
        list($tableName) = explode(' ', $tableName);
        $result          = $this->query('PRAGMA table_info( ' . $tableName . ' )');
        $info            = array();
        if ($result) {
            foreach ($result as $key => $val) {
                $info[$val['name']] = array(
                    'name'    => $val['name'],
                    'type'    => $val['type'],
                    'notnull' => (bool) (1 === $val['notnull']),
                    'default' => $val['dflt_value'],
                    'primary' => '1' == $val['pk'],
                    'autoinc' => false,
                );
            }
        }
        return $info;
    }

    /**
     * 取得数据库的表信息
     * @access public
     * @return array
     */
    public function getTables($dbName = '')
    {
        $result = $this->query("SELECT name FROM sqlite_master WHERE type='table' "
            . "UNION ALL SELECT name FROM sqlite_temp_master "
            . "WHERE type='table' ORDER BY name");
        $info = array();
        foreach ($result as $key => $val) {
            $info[$key] = current($val);
        }
        return $info;
    }

    /**
     * SQL指令安全过滤
     * @access public
     * @param string $str  SQL指令
     * @return string
     */
    public function escapeString($str)
    {
        return str_ireplace("'", "''", $str);
    }

    /**
     * limit
     * @access public
     * @return string
     */
    public function parseLimit($limit)
    {
        $limitStr = '';
        if (!empty($limit)) {
            $limit = explode(',', $limit);
            if (count($limit) > 1) {
                $limitStr .= ' LIMIT ' . $limit[1] . ' OFFSET ' . $limit[0] . ' ';
            } else {
                $limitStr .= ' LIMIT ' . $limit[0] . ' ';
            }
        }
        return $limitStr;
    }

    /**
     * 随机排序
     * @access protected
     * @return string
     */
    protected function parseRand()
    {
        return 'RANDOM()';
    }
}
```