---
title: 二分查找的增长阶数
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

https://www.coursera.org/learn/algorithms-part1/lecture/Xk03a/order-of-growth-classifications

### 原题


Which of the following order-of-growth classifications represents the maximum number of array accesses used to binary search a sorted of length n?

<!--more-->



A. constant


B. logarithmic


C. linear


D. linearithmic

### 读题


下列哪个增长阶数的分类表示用于二分查找长度为n的数组访问的最大值？

### 解题


在二分查找中，数组访问的次数和比较的次数相等，大约为`1 lgN`，所以答案为B 对数级别。
