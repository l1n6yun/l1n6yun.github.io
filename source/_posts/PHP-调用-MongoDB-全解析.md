title: PHP 调用 MongoDB 全解析
author: l1n6yun
tags: 
 - PHP
 - MongoDB
 - 后端开发
 - 软件开发
categories: []
date: 2024-10-19 11:22:00
---
# PHP 调用 MongoDB 全解析

MongoDB 是一款流行的开源文档型数据库，它以 JSON 风格的文档形式存储数据，具有高性能、高可扩展性和灵活的数据模型等特点。而 PHP 作为一种广泛应用于 Web 开发的脚本语言，与 MongoDB 结合可以为开发者提供强大的数据存储和处理能力。本文将详细介绍如何使用 PHP 调用 MongoDB 进行数据的增删改查等操作。

## 环境准备

在使用 PHP 调用 MongoDB 之前，需要确保已经安装了 MongoDB 数据库和 PHP 的 MongoDB 扩展。可以使用 Composer 来安装 MongoDB 的 PHP 驱动，在项目根目录下创建一个`composer.json`文件，内容如下：

json

```json
{
    "require": {
        "mongodb/mongodb": "^1.13"
    }
}
```

然后在终端中执行`composer install`命令来安装依赖。

## 代码示例与解析

### 连接 MongoDB

```php
<?php

require 'vendor/autoload.php';

$client = new MongoDB\Client("mongodb://localhost:27017");
//指定数据库
$db = $client->selectDatabase('test');
//指定集合
$collection = $db->selectCollection('users');

// 另一种指定合集的方式
$collection = $client->test->users;
```

上述代码首先通过`require 'vendor/autoload.php';`引入 Composer 的自动加载文件，然后使用`MongoDB\Client`类来连接本地的 MongoDB 服务器，端口为`27017`。接着指定了要使用的数据库`test`和集合`users`。

### 插入数据

#### 插入单条数据

```php
$collection->insertOne([
    'name' => '张三',
    'age' => 18,
    'sex' => '男'
]);
```

使用`insertOne`方法可以向集合中插入一条数据，数据以关联数组的形式传入。

#### 指定 ID 插入数据

```php
$collection->insertOne([
    '_id' => new MongoDB\BSON\ObjectID('5e7b0b0b0b0b0b0b0b0b0b0b'),
    'name' => '李四',
    'age' => 27,
    'sex' => '男'
]);
```

在插入数据时，可以手动指定`_id`字段，使用`MongoDB\BSON\ObjectID`类来创建一个 ObjectID 对象。

#### 插入多条数据

```php
$collection->insertMany([
    [
        'name' => '王五',
        'age' => 18,
        'sex' => '男'
    ],
    [
        'name' => '赵六',
        'age' => 18,
        'sex' => '男'
    ]
]);
```

使用`insertMany`方法可以一次性插入多条数据，传入一个二维数组，每个子数组代表一条数据。

### 查询数据

#### 查询单条数据

```php
$document = $collection->findOne([
    'name' => '张三'
]);
var_dump(json_encode($document,  JSON_UNESCAPED_UNICODE));
```

使用`findOne`方法可以查询符合条件的第一条数据，返回一个文档对象，使用`json_encode`方法将其转换为 JSON 字符串并输出。

#### 查询多条数据

```php
$cursor = $collection->find([
    'name' => '张三'
]);
foreach ($cursor as $document) {
    var_dump(json_encode($document,  JSON_UNESCAPED_UNICODE));
}
```

使用`find`方法可以查询符合条件的多条数据，返回一个游标对象，通过`foreach`循环遍历游标对象，输出每条数据。

#### 查询指定数量的数据

```php
$cursor = $collection->find([
    'name' => '张三'
], [
    'limit' => 2
]);
foreach ($cursor as $document) {
    var_dump(json_encode($document,  JSON_UNESCAPED_UNICODE));
}
```

在`find`方法的第二个参数中，可以使用`limit`选项来指定查询结果的数量。

#### 条件查询

```php
// 查询年龄大于18的数据
$cursor = $collection->find([
    'age' => [
        '$gt' => 18
    ]
]);
foreach ($cursor as $document) {
    var_dump(json_encode($document,  JSON_UNESCAPED_UNICODE));
}

// 查询年龄小于18，名称等于张三的数据
$cursor = $collection->find([
    'name' => '张三',
    'age' => [
        '$lt' => 19
    ]
]);
foreach ($cursor as $document) {
    var_dump(json_encode($document,  JSON_UNESCAPED_UNICODE));
}

// 查询性别为空的数据
$cursor = $collection->find([
    'sex' => [
        '$exists' => false
    ]
]);
foreach ($cursor as $document) {
    var_dump(json_encode($document,  JSON_UNESCAPED_UNICODE));
}
```

在查询条件中，可以使用 MongoDB 的查询操作符，如`$gt`（大于）、`$lt`（小于）、`$exists`（是否存在）等。

#### 排序查询

```php
$cursor = $collection->find([
    'name' => '张三'
], [
    'sort' => [
        'age' => 1    // 1正序，-1倒叙
    ]
]);
foreach ($cursor as $document) {
    var_dump(json_encode($document,  JSON_UNESCAPED_UNICODE));
}
```

在`find`方法的第二个参数中，可以使用`sort`选项来对查询结果进行排序，`1`表示正序，`-1`表示倒序。

### 修改数据

#### 查询并修改

```php
$cursor = $collection->findOneAndUpdate([
    'name' => '张三'
], [
    '$set' => [
        'age' => 27
    ]
]);
var_dump(json_encode($cursor,  JSON_UNESCAPED_UNICODE));
```

使用`findOneAndUpdate`方法可以查询符合条件的第一条数据并进行修改，返回修改前的文档对象。

#### 更新数据

```php
$collection->updateOne([
    'name' => '张三'
], [
    '$set' => [
        'age' => 27
    ]
]);
```

使用`updateOne`方法可以更新符合条件的第一条数据，使用`$set`操作符来指定要更新的字段和值。

#### 查询并替换

```php
$cursor = $collection->findOneAndReplace([
    'name' => '张三'
], [
    'name' => '李四',
    'age' => 27
]);
var_dump(json_encode($cursor,  JSON_UNESCAPED_UNICODE));
```

使用`findOneAndReplace`方法可以查询符合条件的第一条数据并进行替换，返回替换前的文档对象。

#### 存在更新不存在插入

```php
$collection->updateOne([
    'name' => '张三'
], [
    '$set' => [
        'age' => 27
    ]
], [
    'upsert' => true
]);
```

在`updateOne`方法的第三个参数中，使用`upsert`选项设置为`true`，可以实现如果符合条件的数据存在则更新，不存在则插入的功能。

#### 更新多条数据

```php
$collection->updateMany([
    'name' => '张三'
], [
    '$set' => [
        'age' => 27
    ]
]);
```

使用`updateMany`方法可以更新符合条件的多条数据。

### 删除数据

#### 删除单条数据

```php
$collection->deleteOne([
    'name' => '张三'
]);
```

使用`deleteOne`方法可以删除符合条件的第一条数据。

#### 删除多条数据

```php
$collection->deleteMany([
    'name' => '张三'
]);
```

使用`deleteMany`方法可以删除符合条件的多条数据。

### 删除集合和数据库

```php
// 删除集合
$collection->drop();

// 删除数据库
$db->drop();
```

使用`drop`方法可以删除集合和数据库。
