---
title: Java 流的使用
date: 2022-01-03
tags:
  - Java
---

## 流的创建

 ```java
 // 从Collection和数组中创建
 Collection.stream();
 Collection.parallelStream();
 Arrays.stream(T array);
 Stream.of();
 // 从BefferReader中创建
 java.io.BufferedReader.lines();
 // 静态工厂
 java.util.stream.IntStream.range();
 java.nio.file.Files.walk();
 // 自己构建
 java.util.Spliterator
```

## 流的常用操作

### Intermediate 操作

🕢 lazy

包含的操作有：map (mapToInt, flatMap 等)、 filter、 distinct、 sorted、 peek、 limit、 skip、 parallel、 sequential、 unordered

### Terminal 操作

🕢 一个流只能有一个Terminal操作，执行一次后流就用光了。

forEach、 forEachOrdered、 toArray、 reduce、 collect、 min、 max、 count、 anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 iterator

### short-circuiting 操作

🕢 主要是针对无限的流 转换为 有限的结果。

anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 limit

## 流的典型用法

### map/flatMap

转换为大写

平方数

一对多

### filter

留下偶数

把单词挑出来

### foreach

打印姓名

### peek

打印 > 转换大写 > 打印 > 返回 list

### reduce

🕢 理解为对规则的作用，像 sum min max 这一类就是一种特殊的 reduce。

字符串连接

求最小值

求和

### sorted

根据函数参数进行排序

### min/max/distinct

找出文件中最长一行的长度

## 进阶：自己生成流

### Stream.generate

生成10个随机的整数

### Stream.iterate

生产一个等差数列
