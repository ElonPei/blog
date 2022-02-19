---
categories:
  - Java
created_time: February 19, 2022 9:30 AM
date: 2018/03/10
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: JVM垃圾收集器知识总结
---


> 本文大部分的知识来自于周志明大神的《深入理解Java虚拟机（第2版）》一书，系统学习JVM相关知识的强烈推荐。
> 

# 一、垃圾收集算法

有关垃圾收集算法，有一个国外的大神用JS生成的动图直观的展示了垃圾回收的过程，有助于对垃圾收集算法的理解，这是文章[连接](https://spin.atomicobject.com/2014/09/03/visualizing-garbage-collection-algorithms/)。

## 无垃圾回收(NO-GC)

在程序运行结束后，一次性的对内存进行回收，是最简单的回收机制。其并发性能最高，如果能把任务的颗粒度变得更细，可以有效的清理垃圾。最大的缺点是对长时间运行的程序或者占用内存大的程序来说，非常容易内存溢出。

## 引用计数算法(Reference Counting)

每个对象都持有一个引用的计数器，每当有一个地方引用它时，计数器就加一，当引用失效时，计数器就减一，当计数器为零时，说明对象是可被回收的。引用计数器最大的缺陷时循环引用问题。而且在并发情况下，引用计数器存在线程安全问题。

## 标记清除算法(Mark Sweep)

顾名思义，标记清除算法分为两个阶段，『标记』和『清除』。相对于引用计数算法，解决了循环引用的问题，并且开销要小，因为不需要维护计数器了。

标记清除算法会产生内存碎片，从而导致大对象无法获取到连续的内存空间。并且，两个阶段的执行效率都不高。

## 标记整理算法(Mark Compact)

标记整理算法是在标记清除算法的基础上进行改进的算法，标记过程和标记清除算法一下，标记后，会把所有对象向一端移动，然后清除其余内存。

## 复制算法(Copying)

复制算法在针对对象存活率较低的场景下有较高的效率，复制算法会降低空间成本。在Java虚拟机堆内存新生代非常适合使用这种算法。

# Java垃圾收集器

Java垃圾收集器使用分代收集算法，由于垃圾收集主要集中在堆内存中，于是把堆分为“年轻代”和“年老代”，也把方法区在逻辑上称为“永久代”。年轻代中的对象存活时间较短，绝大部分垃圾收集器使用复制算法。年老代中对象存活时间较长，一般采用标记-清除或者标记-整理算法。下图为各个收集器是否

![http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/gc_collector.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/gc_collector.jpg)

gc_collector.jpg

## 年轻代中的垃圾收集器

### Serial收集器

- 采用复制算法实现
- 单线程
- GC线程执行时，必须 Stop The World

### ParNew收集器

- 采用复制算法实现
- 多线程
- 多条GC线程并行执行，用户线程必须 Stop The World

### Parallel Scavenge收集器

- 采用复制算法实现
- 并行多线程收集器
- 多条GC线程并行执行，用户线程必须 Stop The World
- 其他收集器关注用户线程停顿时间，此收集器关注程序吞吐量。

## 年老代

### Serial Old收集器

- Serial Old收集器时Serial收集器的年老代版本
- 单线程收集器
- 标记-整理算法
- 是CMS算法的后备方案

### Parallel Old收集器

- Parallel Old是Parallel Scavenge收集器的老年代版本
- 多线程收集器
- 注重吞吐量以及CPU资源敏感场合，可以优先考虑Parallel Scavenge + Parallel Old的组合。

### CMS收集器

- CMS(Concurrent Mark Sweep)收集器是一种以获取最短停顿时间目标的收集器。
- 使用标记清除算法实现
- 多线程

**优点**：

- 并发收集
- 低停顿

**缺点**：

- CMS收集器对CPU资源非常敏感（回收线程数 (cpu数量 + 3)/4 所以CPU不足4个时，垃圾收集线程占用50% CPU资源 ）
- CMS收集器无法处理浮动垃圾(Floating Garbage)
- 标记-清除算法带来的内存碎片处理

**CMS过程：**

![http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/CMS-Process.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/CMS-Process.jpg)

CMS-Process.jpg

# remark

由于G1收集器暂时使用并不主流，暂时不做过多的学习。