---
title: JVM内存模型简述
date: 2018-03-01
tags:
  - Java
---


> 本文大部分的知识来自于周志明大神的《深入理解Java虚拟机（第2版）》一书，系统学习JVM相关知识的强烈推荐。

Java内存区域概括性的来讲，分为线程独占的虚拟机栈(VM Stack)、本地方法栈(Native Method Stack)、程序计数器(Programer Counter Register)和线程共享的方法区(Method Area)、堆(Heap)。

![http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/Xnip2018-03-71_15-54-12.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/Xnip2018-03-71_15-54-12.jpg)

## 一、线程独占区域

### 程序计数器

程序计数器是一块较小的内存区域，每一个线程都会分配一个程序计数器，用来存储当前所致行的字节码的行号，程序的执行、跳转、循环、异常处理都需要这个计数器来完成。它的生命周期和线程相同。

### 虚拟机栈

和程序计数器一样，虚拟机栈也是线程独占的，生命周期和线程相同，虚拟机栈描述的是Java方法执行的内存模型：每个方法执行的同时都会创建一个栈帧（Stack Frame）用于存储局部变量表、操作数栈、动态链接、方法出口等信息。

每一个方法从调用到执行完成的过程，对应着一次入栈和出栈的过程。

### 局部变量表

局部变量表中存储着编译器可知的基本数据类型、对象引用和returnAddress类型(指向一条字节码指令的地址)。

局部变量表的大小表示为局部变量空间(Slot)，64位长度的double和long占用两个Slot，其余占用一个。

### 本地方法栈

本地方法栈和虚拟机栈的作用类似，本地方法栈为虚拟机使用到的Native方法服务，虚拟机规范没有明确规范此区域，Sun HotSpot直接把本地方法栈和虚拟机栈合二为一。

## 二、线程共享区域

### 方法区

方法区是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息，常量，静态变量，即时编译器编译后的代码等数据。

HotSpot虚拟机中，方法区也称为“永久代(Permanent Generation)”。

### 运行时常量池

运行时常量池是方法区的一部分，用于存放编译期间生成的各种字面量和符号引用，类加载后进入方法区的运行时常量池中存放。

### 堆

堆是Java虚拟机内存模型中最大的一块线程共享的区域，此内存区域的唯一作用就是存放对象实例。

Java堆是垃圾收集器管理的主要区域，也称为“GC堆”。使用分代收集算法，分为新生代和老年代。从内存分配的角度看，Java堆可能划分出多个线程私有的分配缓冲区(Thread Local Allocation Buffer, TLAB)。

---

以上，Java内存模型简述完毕，上一张总结的思维导图。

![http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/WechatIMG178.jpeg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/WechatIMG178.jpeg)
