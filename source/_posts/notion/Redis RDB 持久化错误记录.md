---
categories:
  - Java
date: 2029/01/03
excerpt: 在 Java 客户端输出的日志上，报错的意思是无法在磁盘进行RDB快照持久化，但是在监控系统中 Redis
  所在服务器磁盘并没有压力，可以排除磁盘满而无法进行持久化。
show_category: Yes
status: 待发布
tags:
  - Java
  - Redis
urlname: Reids RDB 持久化
title: Redis RDB 持久化错误记录
---


# Redis RDB 持久化错误记录

## 问题描述

Java process log output:

```
MISCONF Redis is configured to save RDB snapshots, but it is currently not able to persist on disk.
Commands that may modify the data set are disabled, because this instance is configured to report errors during writes if RDB snapshotting fails (stop-writes-on-bgsave-error option). Please check the Redis logs for details about the RDB error.; nested exception is redis.clients.jedis.exceptions.JedisDataException: MISCONF Redis is configured to save RDB snapshots, but it is currently not able to persist on disk. Commands that may modify the data set are disabled, because this instance is configured to report errors during writes if RDB snapshotting fails (stop-writes-on-bgsave-error option). Please check the Redis logs for details about the RDB error.
```

Redis log output：

```
5459:M 25 Jun 11:05:55.025 * 1 changes in 900 seconds. Saving...
5459:M 25 Jun 11:05:55.025 # Can't save in background: fork: Cannot allocate memory
```

The monitoring system:

![](/notion_images/dd545e2572058d51acd4b934f85a43f4.png)

https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e53a0914-982f-47e3-adf5-669414eae34c/Untitled.png

![](/notion_images/0d4acf7a5ee4365a75126118e78ddb92.png)

https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7716e658-b3e4-44c9-84a1-ff67fb9de3c5/Untitled.png

红色线为 Redis Server 端，蓝色线为连接 Redis 的客户端。可以看到，客户端硬盘和内存的压力都很大，服务端内存也只剩余大约 30% 。

## 问题分析

在 Java 客户端输出的日志上，报错的意思是无法在磁盘进行RDB快照持久化，但是在监控系统中 Redis 所在服务器磁盘并没有压力，可以排除磁盘满而无法进行持久化。

根据 Redis 的报错 `Can't save in background: fork: Cannot allocate memory`信息，查看 Redis 相关文档，此报错原因试因为 `stop-writes-on-bgsave-error yes` 参数而产生的报错，在RDB持久化的进程执行持久化时，Redis 不允许用户进行任何更新操作，与 Java 中的错误日志吻合。

## 解决方法

### 1. 修改Reids参数 `stop-writes-on-bgsave-error false`

在 RDB持久化 出错时，允许用户继续操作。这种方式并没有解决真正的报错，我们来看第二种方式。

### 2. 修改内核参数，在 `/etc/sysctl.conf` 添加 `vm.overcommit_memory=1`，使用`sysctl -p`刷新生效。

如图，修改后，Redis自动恢复。

![](/notion_images/92bea9a5bfe09be1fd54f424ba560699.png)

https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee7a4f74-e9e3-458d-a969-504842841fc5/Untitled.png

下面我们来分析一下这个参数

### 首先，这个参数涉及到了在什么情况下，Linux会内存不足？

Linux 下 CommitLimit 的大小用来限制用户态可使用的内存资源，通过命令`grep -i commit /proc/meminfo`，`CommitLimit`表示内存分配上限，`Committed-AS`表示已经分配的内存大小。

```
CommitLimit = 物理内存 * overcommit_ratio(/proc/sys/vm/overcommit_ratio) + swap
```

当满足下方公式时，系统就会认为内存不足。

```
应用程序申请内存 + 系统已经分配的内存(Committed-AS) > CommitLimit
```

### 其次，当满足内存不足时，操作系统根据`overcommit_memory`参数来执行不同的策略。

- vm.overcommit_memory = 0 启发策略

比较 `请求分配的内存 > 系统空闲内存 + swap` 如果满足，则虚拟地址空间分配失败。

- vm.overcommit_memory = 1 允许 overcommit

直接放行，不进行任何校验，任何情况下系统都会为应用程序分配虚拟内存空间。 **弊端**：完全屏蔽了应用程序对系统物理内存的感知，当发生缺页中断的机制来进行分配内存，一旦内存分配失败，会引起系统OOM机制杀进程。

- vm.overcommit_memory = 2 禁止 overcommit

这种情况下系统所能分配的内存不会超过上面提到的 `CommitLimit` 大小，如果资源用光，任何尝试分配内存的行为都会失败，也就是说不能运行任何新的进程。

### 最后，对 Redis 产生了什么影响

Redis 异步 BGSAVE 命令，主进程 fork 后，复制自身并通过这个新的进程来持久化到磁盘，完毕后进程自动关闭。

如果Redis分配的内存太大，在 fork 进程的时候，容易发生内存不足无法fork的情况。

## 思考

1. 我们平时在使用Rdis的时候，一定要记得给缓存设置有效时间，避免非热点数据占用内存。
2. Redis的RDB快照持久化需要fork新的进程，在内存占用大时，fork的代价极大。