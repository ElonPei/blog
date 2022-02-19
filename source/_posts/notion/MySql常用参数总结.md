---
categories:
  - 数据库
created_time: February 19, 2022 9:30 AM
date: 2018/08/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: MySql常用参数总结
---


查看innodb引擎状态

```
SHOW ENGINE INNODB STATUS;
```

查看mysql版本号

```
SELECT VERSION();
```

回收使用已经使用并分配的undo页

```
SHOW VARIABLES LIKE 'innodb_purge_threads';
```

### 缓冲池

查看缓冲池的大小 单位为字节 (Byte)

```
SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
```

查看缓冲池实例个数

```
SHOW VARIABLES LIKE 'innodb_buffer_pool_instances';
```

查看各个缓存池的使用状态

```
SELECT `POOL_ID`,`POOL_SIZE`,`FREE_BUFFERS`,`DATABASE_PAGES`,`HIT_RATE`,`PAGES_MADE_YOUNG`,`PAGES_NOT_MADE_YOUNG` FROM `INNODB_BUFFER_POOL_STATS`;
```

LRU算法中 midpoint 位置

```
SHOW VARIABLES LIKE 'innodb_old_blocks_pct';
```

页读取到mid位置后，等待多久被加入到LRU列表的热端​

```
SHOW VARIABLES LIKE 'innodb_old_blocks_time';
```

观察LRU列表中每个页的具体信息

```
SELECT `TABLE_NAME`,`SPACE`,`PAGE_NUMBER`,`PAGE_TYPE` FROM `INNODB_BUFFER_PAGE_LRU`;
```

```
SELECT COUNT(*) FROM INNODB_BUFFER_PAGE_LRU;
```

Flush 列表脏页数量

```
SELECT `TABLE_NAME`,`SPACE`,`PAGE_NUMBER`,`PAGE_TYPE` FROM `INNODB_BUFFER_PAGE_LRU` WHERE `OLDEST_MODIFICATION` > 0;
```

查看重做日志缓冲大小

```
SHOW VARIABLES LIKE 'innodb_log_buffer_size';
```

LRU列表中可用页的数量（如果数量不足，Innodb存储引擎会将LRU列表尾端的页移除）

```
SHOW VARIABLES LIKE 'innodb_lru_scan_depth';
```

缓冲池中的脏页checkpoint阀值（超过此值进行checkpoint）

```
SHOW VARIABLES LIKE 'innodb_max_dirty_pages_pct';
```

当前缓冲池中脏页的比例

```
SHOW VARIABLES LIKE 'buf_get_modified_ratio_pct';
```

控制刷新到磁盘脏页的比例

```
SHOW VARIABLES LIKE 'innodb_io_capacity';
```

> 在合并插入缓冲时，合并插入缓冲的数量为innodb_io_capacity的5%
> 
> 
> 在从缓冲区刷新脏页时，刷新脏页的数量为innodb_io_capacity
> 

控制MasterThread每次回收Undo也得数量

```
SHOW VARIABLES LIKE 'innodb_purge_batch_size';
```

是否开启自适应刷新(脏页)

```
SHOW VARIABLES LIKE 'innodb_adaptive_flushing';
```

### Innodb逻辑存储结构相关字段

设置默认页的大小

```
show variables like 'innodb_page_size';
```