---
title: 估算规模为n的三层嵌套循环代码中数组访问次数
date: 2018-01-01
---

本题为《算法4》作者 Robert Sedgewick 和 Kevin Wayne 在 Cursera 上开设的公开课的习题解答，本题为WEEK1中第二部分视频中的选择题。地址为：

https://www.coursera.org/learn/algorithms-part1/lecture/nrqSL/mathematical-models


### 原题


How many array accesses does the following code fragment make as a function of n?

(Assume the compiler does not optimize away any array accesses in the innermost loop.)

<!--more-->


`java
t sum = 0;
r(int i = 0; i < n; i++)
or(int j = i+1; j < n; j++)
for(int k = 1; k < n; k=k*2)
	if(a[i] + a[j] >= a[k])
		sum++;
```


A. $ \sim3n^2 $


B. $ \sim\frac{3}{2}n^2lgn $


C. $ \sim\frac{3}{2}n^3 $


D. $ \sim3n^3 $


### 读题


以下代码中作为规模为 n 的函数数组访问的次数大约为多少？（假设编译器不会优化在最内层循环的数组访问性能）


### 解题


1. 计算出所有 `for循环`的执行频率

2. 根据频率，得到` k 循环`的被执行次数为$\sim\frac{1}{2}N^2$，`k循环`自身的执行次数为$\sim lgN$，故数组的访问次数为：$$\sim\frac{1}{2}N^2\times lgN\times3 = \sim\frac{3}{2}N^2lgN$$故答案为 B。
