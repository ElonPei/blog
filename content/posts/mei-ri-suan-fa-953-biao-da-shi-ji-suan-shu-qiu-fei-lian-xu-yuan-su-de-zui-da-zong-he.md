---
title: 每日算法 953 表达式计算树求非连续元素的最大总和
date: 2021-08-03
tags:
  - Algorithm
---


> 思路参考：stackoverflow

# 原题说明


Given a list of integers, write a function that returns the largest sum of non-adjacent numbers. Numbers can be `0` or negative.


For example, `[2, 4, 6, 2, 5]` should return `13`, since we pick `2`, `6`, and `5`. `[5, 1, 1, 5]` should return `10`, since we pick `5` and `5`.


Follow-up: Can you do this in O(N) time and constant space?

# 思路


这是一道典型的动态规划来解决的问题，假设A是给定的数组arr，另一个数组Sum表示从arr[0]…arr[i]中非连续元素的最大和。


```
Sum[0] = arr[0]
Sum[1] = max(Sum[0], arr[1])
Sum[2] = max(Sum[0] + arr[2], Sum[1])
...
Sum[i] = max(Sum(i-2) + arr[i], Sum[i-1]) when i>=2
```

# 代码


```java
public static int getSum(int[] arr, int n) {
int[] sum = new int[n];
for (int i = 0; i < arr.length; i++) {
  if (i == 0) {
      sum[0] = arr[0];
  } else if (i == 1) {
      sum[1] = Math.max(arr[0], arr[1]);
  } else {
      sum[i] = Math.max(sum[i - 2] + arr[i], sum[i - 1]);
  }
}
return sum[n - 1];
}
```
