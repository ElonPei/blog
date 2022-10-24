---
title: 根据不同规模样本计算程序运行时间公式
date: 2017-11-22
tags:
  - Algorithm
---

本题为《算法4》作者 Robert Sedgewick 和 Kevin Wayne 在 Cursera 上开设的公开课的习题解答，本题为WEEK1中第二部分视频中的选择题。地址为：

[点此查看](https://www.coursera.org/learn/algorithms-part1/lecture/jmkCT/observations)


### 原题


Suppose that you make the following observations of the running time T(n)(in seconds) of a program as a function of the input size n. Which of the following functions best models the running time T(n)?



A. $ 3.3\times10^{-4} \times n $


B. $ n^2 $


C. $ 5.0 \times 10^{-9} \times n^2 $


D. $ 6.25 \times 10^{-9} \times n^2 $


### 读题


假设下表为程序中一个函数的输入规模为n与观察到的运行时间T(n)的对应关系，下列哪一个函数模型最接近运行时间T(n)。


### 解题


1. 对实验数据进行分析

2. 在双对数坐标中，易得一条斜率为2的直线，这种规律符合`幂次法则`，公式如下：$$ T(n) = aN^b $$代入数据求：$$ a=\frac{T(n)}{N^2}=\frac{20.5}{64000^2}\approx5.0\times10^{-9} $$故答案为 C。
