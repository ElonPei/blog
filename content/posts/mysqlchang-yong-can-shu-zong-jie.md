---
title: MySql常用参数总结
date: 2018-08-01
---

查看innodb引擎状态


```mysql
SHOW ENGINE INNODB STATUS;
```


查看mysql版本号


```mysql
SELECT VERSION();
```


回收使用已经使用并分配的undo页

```mysql
SHOW VARIABLES LIKE 'innodb_purge_threads';
```


<!-- more -->


### 缓冲池


查看缓冲池的大小 单位为字节 (Byte)


```mysql
SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
```


查看缓冲池实例个数


```mysql
SHOW VARIABLES LIKE 'innodb_buffer_pool_instances';
```


查看各个缓存池的使用状态


```mysql
SELECT `POOL_ID`,`POOL_SIZE`,`FREE_BUFFERS`,`DATABASE_PAGES`,`HIT_RATE`,`PAGES_MADE_YOUNG`,`PAGES_NOT_MADE_YOUNG` FROM `INNODB_BUFFER_POOL_STATS`;
```


LRU算法中 midpoint 位置

```mysql
SHOW VARIABLES LIKE 'innodb_old_blocks_pct';
```


页读取到mid位置后，等待多久被加入到LRU列表的热端​

```mysql
SHOW VARIABLES LIKE 'innodb_old_blocks_time';
```


观察LRU列表中每个页的具体信息

```mysql
SELECT `TABLE_NAME`,`SPACE`,`PAGE_NUMBER`,`PAGE_TYPE` FROM `INNODB_BUFFER_PAGE_LRU`;
```

```mysql
SELECT COUNT(*) FROM INNODB_BUFFER_PAGE_LRU;
```


Flush 列表脏页数量


```mysql
SELECT `TABLE_NAME`,`SPACE`,`PAGE_NUMBER`,`PAGE_TYPE` FROM `INNODB_BUFFER_PAGE_LRU` WHERE `OLDEST_MODIFICATION` > 0;
```


查看重做日志缓冲大小


```mysql
SHOW VARIABLES LIKE 'innodb_log_buffer_size';
```


LRU列表中可用页的数量（如果数量不足，Innodb存储引擎会将LRU列表尾端的页移除）


```mysql
SHOW VARIABLES LIKE 'innodb_lru_scan_depth';
```


缓冲池中的脏页checkpoint阀值（超过此值进行checkpoint）


```mysql
SHOW VARIABLES LIKE 'innodb_max_dirty_pages_pct';
```


当前缓冲池中脏页的比例


```mysql
SHOW VARIABLES LIKE 'buf_get_modified_ratio_pct';
```


控制刷新到磁盘脏页的比例


```mysql
SHOW VARIABLES LIKE 'innodb_io_capacity';
```


> 在合并插入缓冲时，合并插入缓冲的数量为innodb_io_capacity的5%

>

> 在从缓冲区刷新脏页时，刷新脏页的数量为innodb_io_capacity


控制MasterThread每次回收Undo也得数量


```mysql
SHOW VARIABLES LIKE 'innodb_purge_batch_size';
```


是否开启自适应刷新(脏页)


```mysql
SHOW VARIABLES LIKE 'innodb_adaptive_flushing';
```


### Innodb逻辑存储结构相关字段


设置默认页的大小


```mysql
show variables like 'innodb_page_size';
```
