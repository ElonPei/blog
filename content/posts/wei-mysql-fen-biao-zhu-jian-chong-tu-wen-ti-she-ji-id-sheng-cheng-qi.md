---
title: 为 MySQL 分表主键冲突问题设计 ID 生成器
date: 2019-04-01
---


分表业务场景下，我们需要保证数据库主键id唯一，在并发要求不是很高的场景下，我们可以利用数据库自增id的特性来做一个id生成器。

## 数据库建表语句


```sql
CREATE TABLE `sequence` (
`id` int(11) NOT NULL AUTO_INCREMENT,
`stub` char(1) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8 COMMENT='主键id发号器';
```

## ID生成语句


可以使用任何语言来实现，保证执行的两个sql处于同一个事务即可，如果不在同一个事务，多线程下会产生竞态条件。


```sql
REPLACE INTO `sequence` (stub) VALUES ('o');
SELECT LAST_INSERT_ID();
```


这样我们就得到了一个唯一自增的ID，利用这个id来插入MySQL主键非常合适，有序自增对索引也友好。如果利用雪花算法、uuid等，对索引不友好，影响索引的性能。
