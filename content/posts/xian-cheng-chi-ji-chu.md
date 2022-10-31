---
title: 线程池基础
date: 2021-11-30
tags:
  - Java
---


## 线程池是什么？

线程池是一种基于池化思想管理线程的工具。

## 使用线程池有什么好处?

- 降低资源消耗

- 提高响应速度

- 提高线程的可管理性

- 提交拓展性（比如定时执行等）

## 线程池解决的问题是什么？

- 频繁申请销毁资源，会带来巨大的资源损耗。

- 无法限制资源的无限申请，带来资源溢出风险。

- 系统无法合理管理内部资源，降低了系统的稳定性。

## 池化思想的其他应用场景

- 内存池

- 连接池

- 对象池

## 关于线程池的理解

线程池整体流程

![线程池整体流程](https://raw.githubusercontent.com/peiel/oss/master/uPic/QeZ4VK.jpg)

### 线程池如何维护自身状态？

![线程池的生命周期](https://raw.githubusercontent.com/peiel/oss/master/uPic/Cke0my.jpg)

```java
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
```

### 线程池如何管理任务？

#### 任务调度

任务调度流程

![任务调度流程](https://raw.githubusercontent.com/peiel/oss/master/uPic/sE7lun.jpg)

#### 任务缓存

任务会通过一个阻塞队列来进行缓存任务

#### 任务申请

任务的申请会触发 `getTask()` 方法来获取一个任务。

#### 任务的拒绝

线程池有一个最大的容量，当队列中任务满了的时候，会触发拒绝机制，拒绝机制中会有具体的拒绝策略。

### 线程池如何管理线程？

#### worker线程

```java
private final class Worker extends AbstractQueuedSynchronizer implements Runnable{
  final Thread thread;//Worker持有的线程
  Runnable firstTask;//初始化的任务，可以为null
}
```

#### worker线程的添加

添加worker线程，使用 `addWorker()` 方法来添加。

#### worker线程的回收

worker线程的回收是在 `processWorkerExit()` 方法中进行的。

#### worker线程如何来执行任务

worker线程通过调用 `runWorker()` 方法来执行任务。

Worker是通过继承AQS，使用AQS来实现独占锁这个功能，这里用到了非重入锁的概念，可以重点看一下。
