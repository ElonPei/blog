---
title: nlgn 与 n 的平方执行效率比较
date: 2017-12-01
tags:
  - Algorithm
---


<script type="text/x-mathjax-config">

MathJax.Hub.Config({

tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}

});

</script>


<script type="text/javascript" async

src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">

</script>


本题为《算法4》作者 Robert Sedgewick 和 Kevin Wayne 在 Cursera 上开设的公开课的习题解答，本题为WEEK1中第二部分视频中的选择题。地址为：

https://www.coursera.org/learn/algorithms-part1/lecture/xaxyP/analysis-of-algorithms-introduction

### 原题

Suppose that n equals 1 million. Approximately how much faster is an algorithm that

performs nlgn operations versus one that performs n² operations？Recall that lg is base-2

logarithm function.

A 20X

B 1,000X

C 50,000X

D 1,000,000X

### 读题

假设 n 等于1百万，执行 nlgn 操作相对于执行 n 的平方操作大约快了多少？回想一下 lg 是以2为底的对数函数。

### 解题

1. 在读懂题目意思的前提下，就是一到很简单的计算题了。

2. 使用公式可得结果 $\frac{1000000^2}{1000000 \cdot log_{2}1000000} \approx \frac{1000000}{20} = 50000$
