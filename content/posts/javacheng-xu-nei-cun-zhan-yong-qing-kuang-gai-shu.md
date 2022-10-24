---
title: Java程序内存占用情况概述
date: 2018-03-01
tags:
  - Java
---



以下内存大小都是Java语言在64位操作系统下的占用情况。


## 基本类型


boolean —> 1bytes

byte —> 1bytes

char —> 2bytes

int —> 4bytes

float —> 4bytes

long —> 8bytes

double —> 8bytes


## 一维数组


char[] —> 2N + 24bytes

int[] —> 4N + 24bytes

double[] —> 8N + 24bytes


## 二维数组


char[][] —> ~2MNbytes

int[][] —> ~4MNbytes

double[][] —> ~8MNbytes


## 对象相关


对象开销 —> 16bytes

引用 —> 8bytes

Padding —> Each object uses a multiple of 8 bytes


## 数据类型值的总内存使用情况


基本数据类型：4byte for int, 8bytes for double, …

对象引用：8bytes

数组：24bytes + 每个数组条目的内存

对象：16bytes + 每个引用变量的内存占用 + 8byte if inner class (for pointer to enclosing class)

Padding：round up to multiple of 8bytes
