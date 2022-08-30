---
title: MySQL使用For Update锁表

date: 2017-06-07 21:53:51
categories:
tags: mysql
---
有时为了保证数据的一致性，需要对数据库进行锁表操作，在Mysql中则可以使用 `FOR UPDATE` 命令锁表，出于对性能的考虑，则需要判断 Mysql 使用 `FOR UPDATE` 命令锁表到底是锁定的整个表，还是仅仅锁定查询记录行。

* 首先测试主键过滤条件的情况，开始事务A：


```sh
mysql> begin;
Query OK, 0 rows affected
mysql> select o.id,o.desc,o.action,o.create_time from oplog o where o.id between 154950 and 154975;
+--------+------+--------+---------------------+
| id     | desc | action | create_time         |
+--------+------+--------+---------------------+
| 154950 | 9999 | 9      | 2017-06-07 10:46:40 |
| 154952 | 9999 | 2      | 2017-06-07 10:18:56 |
| 154953 | 9999 | 2      | 2017-06-07 10:18:56 |
| 154954 | 9999 | 2      | 2017-06-07 10:18:56 |
| 154955 | 9999 | 2      | 2017-06-07 10:18:56 |
| 154956 | 9999 | 2      | 2017-06-07 10:18:56 |
+--------+------+--------+---------------------+
6 rows in set
```

* 接着开始事务B，发现事务B能立刻执行，表明对where条件为主键的情况，是锁记录行，不是锁表：

```sh
mysql> begin;
Query OK, 0 rows affected
mysql> select o.id,o.desc,o.action,o.create_time from oplog o where o.id between 154890 and 154899 for update;
+--------+-------------------------------------------------------------------------------+--------+---------------------+
| id     | desc                                                                          | action | create_time         |
+--------+-------------------------------------------------------------------------------+--------+---------------------+
| 154897 | 746db81780f0                                                                  | 1      | 2017-04-01 09:39:17 |
| 154898 | 9999                                                                          | 2      | 2017-06-07 10:18:56 |
| 154899 | 9                                                                             | 1      | 2017-04-01 09:40:13 |
+--------+-------------------------------------------------------------------------------+--------+---------------------+
3 rows in set
```

* 提交上述两个事物，重新开始事务A：

```sh
mysql> begin;
Query OK, 0 rows affected
mysql> select o.id,o.desc,o.action,o.create_time from oplog o where o.desc='9999' for update;
+--------+------+--------+---------------------+
| id     | desc | action | create_time         |
+--------+------+--------+---------------------+
| 154950 | 9999 | 9      | 2017-06-07 10:46:40 |
| 154952 | 9999 | 2      | 2017-06-07 10:18:56 |
| 154953 | 9999 | 2      | 2017-06-07 10:18:56 |
| 154954 | 9999 | 2      | 2017-06-07 10:18:56 |
| 154955 | 9999 | 2      | 2017-06-07 10:18:56 |
| 154956 | 9999 | 2      | 2017-06-07 10:18:56 |
+--------+------+--------+---------------------+
6 rows in set
```

* 再开始事务B，查询和事务A不一样的条件，结果发现事务B发生了等待：

```sh
mysql> begin;
Query OK, 0 rows affected
mysql> select o.id,o.desc,o.action,o.create_time from oplog o where o.desc='66666' for update;
```

* 再对UNIQUE字段(camp_offer_id)条件进行测试，开始事务A：

```sh
mysql> begin;
Query OK, 0 rows affected
mysql> select g.id,g.camp_offer_id,g.create_time from group g where g.camp_offer_id ='XX03' for update;
+----+---------------+---------------------+
| id | camp_offer_id | create_time         |
+----+---------------+---------------------+
|  7 | XX03          | 2017-03-22 17:43:29 |
+----+---------------+---------------------+
1 row in set
```

* 开始事务B，发现事务B能顺利执行：

```sh
mysql> begin;
Query OK, 0 rows affected
mysql> select g.id,g.camp_offer_id,g.create_time from group g where g.camp_offer_id ='XX04' for update;
+----+---------------+---------------------+
| id | camp_offer_id | create_time         |
+----+---------------+---------------------+
|  8 | XX04          | 2017-03-22 17:43:39 |
+----+---------------+---------------------+
1 row in set
```

* 再不能使用索引条件的语句进行测试，开启事务A：

```sh
mysql> begin;
Query OK, 0 rows affected
mysql> select g.id,g.camp_offer_id,g.create_time from ra_customer_group g where g.camp_offer_id like "%XX%" for update;
+----+---------------+---------------------+
| id | camp_offer_id | create_time         |
+----+---------------+---------------------+
|  5 | XX01          | 2017-03-22 17:43:05 |
|  6 | XX02          | 2017-03-22 17:43:18 |
|  7 | XX03          | 2017-03-22 17:43:29 |
+----+---------------+---------------------+
3 rows in set
```

* 开启查询事务B,发现发生了锁等待：

```
mysql> begin;
Query OK, 0 rows affected
mysql>  select g.id,g.camp_offer_id,g.create_time from ra_customer_group g where g.camp_offer_id = "9000008" for update;
```

通过对一系列的测试对比不难发现，SQL 条件中能使用索引进行数据检索时，则 `FOR UPDATE` 命令使用的是行级锁，当不能使用索引进行数据检索时，`FOR UPDATE` 命令使用的是表锁。

> MySQL 的 InnoDB 引擎只有通过索引条件检索数据，InnoDB 才使用行级锁，否则，InnoDB 将使用表锁！



