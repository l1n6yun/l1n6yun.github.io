---
title: MySQL查询不重复数据
date: 2017-04-08 17:34:01
categories: 
tags: [MySQL]

---
#### 查询不重复数据数量

```mysql
select count(distinct([field name])) from [table name];
```