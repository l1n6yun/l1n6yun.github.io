---
title: Access数据库和MySQL数据库数据去重
description: 生命在于折腾，又把博客折腾到Hexo了。给Hexo点赞。
date: 2017-12-12 11:41:19
categories:
tags: [MySQL,Access]
---

# MySQl数据库删除重复数据保留唯一记录 

```SQL
CREATE TABLE `my_tmp` AS SELECT MIN(`id`) AS `col1` FROM `表名` GROUP BY `字段名`;
DELETE FROM `表名` WHERE `id` NOT IN (SELECT `col1` FROM `my_tmp`);
DROP TABLE `my_tmp`;
```

# Access数据库删除重复数据保留唯一记录

```SQL
DELETE id
FROM `表名`
WHERE `id` NOT IN (SELECT `id` FROM `表名` GROUP BY `字段名`);
```

