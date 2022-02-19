---
categories:
  - 中间件
created_time: February 19, 2022 9:30 AM
date: 2018/01/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Redis设计与实现简要笔记
---


## 字符串实现

简单动态字符串 （Simple Dynamic String)，简称SDS。

```
// 数据结构struct sdshdr{  int len;  int free;  char buf[];};
```

### 与c的字符串的差异

- 查询字符串的时间复杂度不同，c 为 O(n), SDS 为 O(1)
- 对于字符串扩展（拼接），sds相对于c语言，不会出现缓冲区溢出问题。
- 减少修改字符串时带来的内存重分配次数
    
    ​ — SDS实现了空间预分配和惰性空间释放两种优化策略
    
- 二进制安全
- sds可以重用部分<string.h>函数库

## 链表

### 源代码

```
/* * 双端链表节点 */typedef struct listNode {    // 前置节点    struct listNode *prev;    // 后置节点    struct listNode *next;    // 节点的值    void *value;} listNode;
```

```
/* * 双端链表结构 */typedef struct list {    // 表头节点    listNode *head;    // 表尾节点    listNode *tail;    // 节点值复制函数    void *(*dup)(void *ptr);    // 节点值释放函数    void (*free)(void *ptr);    // 节点值对比函数    int (*match)(void *ptr, void *key);    // 链表所包含的节点数量    unsigned long len;} list;
```

### Redis 链表的特性

- 双端：获取某个节点的前置节点和后置节点的时间复杂度都是 O(1)
- 无环
- 带表头指针和表尾指针：通过list结构的head指针和tail指针，程序获取链表的表头节点和表尾节点的时间复杂度为O(1)
- 带链表长度计数器，获取长度的时间复杂度为O(1)
- 多态：使用void(*)指针来保存节点值，通过list结构的属性来设置类型特定函数，所以链表可以用来保存不同类型的值

## 字典

底层实现：一个hash表中有多个hash节点，每个hash节点保存一个键值对。

```
/* * 字典 */typedef struct dict {    // 类型特定函数    dictType *type;    // 私有数据    void *privdata;    // 哈希表    dictht ht[2];    // rehash 索引    // 当 rehash 不在进行时，值为 -1    int rehashidx; /* rehashing not in progress if rehashidx == -1 */    // 目前正在运行的安全迭代器的数量    int iterators; /* number of iterators currently running */} dict;
```

```
/* * 哈希表 * * 每个字典都使用两个哈希表，从而实现渐进式 rehash 。 */typedef struct dictht {    // 哈希表数组    dictEntry **table;    // 哈希表大小    unsigned long size;    // 哈希表大小掩码，用于计算索引值    // 总是等于 size - 1    unsigned long sizemask;    // 该哈希表已有节点的数量    unsigned long used;} dictht;
```

```
/* * 哈希表节点 */typedef struct dictEntry {    // 键    void *key;    // 值    union {        void *val;        uint64_t u64;        int64_t s64;    } v;    // 指向下个哈希表节点，形成链表    struct dictEntry *next;} dictEntry;
```

### hash算法

```
index = dict->type->hashFunction(key)  & dict->ht[x].sizemask;
```

### Rehash 流程

1. 为字典的ht[1]分配内存空间，内存空间的大小取决于要执行的操作，以及ht[0]当前包含的键值对数量（ht[0].used的值）
    - 拓展操作 ht[1]的大小为 >= ht[0].used * 2 的第一个2的n次方的值。
    - 收缩操作 ht[1]的大小为 >= ht[0].used 的第一个2的n次方的值。
2. ht[0] >>rehash>> ht[1]
3. 释放ht[0]，将ht[1]设置为ht[0]，并在ht[1]新创建一个空白hash表

> 其中第二部采用的是渐进式rehash操作
> 

## 整数集合的实现

```
typedef struct intset {    // 编码方式    uint32_t encoding;    // 集合包含的元素数量    uint32_t length;    // 保存元素的数组    int8_t contents[];} intset;
```

特性：整数集合的升级(操作灵活，解决内存)，但是不支持降级。

## 压缩列表

压缩列表是Redis为了节约内存开发的，是由一系列的特殊编码的连续的内存块组成的顺序型数据结构。

## 对象

```
typedef struct redisObject {    // 类型    unsigned type:4;    // 编码    unsigned encoding:4;    // 对象最后一次被访问的时间    unsigned lru:REDIS_LRU_BITS; /* lru time (relative to server.lruclock) */    // 引用计数    int refcount;    // 指向实际值的指针    void *ptr;} robj;
```

### 对象的类型

1. REDIS_STRING
2. REDIS_LIST
3. REDIS_HASH
4. REDIS_SET
5. REDIS_HSET

### 对象的编码

1. REDIS_ENCODING_INT
2. REDIS_ENCODING_EMBSTR (embst编码的简单字符串)
3. REDIS_ENCODING_RAW (SDS)
4. REDIS_ENCODING_HT (字典)
5. REDIS_ENCODING_LINKEDLIST(双端链表)
6. REDIS_ENCODING_ZIPLIST(压缩列表)
7. REDIS_ENCODING_INTSET(整数集合)
8. REDIS_ENCODING_SKIPLIST(跳跃表和字典)