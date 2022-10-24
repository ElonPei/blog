---
title: Two Sum 最优解的解读
date: 2018-07-01
---

LeetCode 上 Two Sum 问题，自己能想到的是双循环的暴力解决法和使用单 Hash Table 来解决。看到有大神写的代码TestCase跑时间消耗为0ms，拜读分析一下。

## 分析

代码如下:（ [点击在LeetCode答案分析中时间消耗排行中查找](https://leetcode.com/problems/two-sum/)）

 ```java
 public int[] twoSum(int[] nums, int target) {
   int mod = 2048 -1;
   int[] map = new int[2048];
   for(int i=0;i<nums.length;i++){
       int key = target - nums[i] & mod;
       if(map[key] != 0){
           int[] ret = {map[key]-1, i};
           return ret;
       }
       map[nums[i]&mod] = i+1;
   }
   throw new IllegalArgumentException("no two sum solution");
 }
```

代码逻辑分析

- 创建了一个2048长度的槽（数组）。

- 对目标值和查找值进行按位与运算得到一个索引值。

- 根据索引值对应的槽来查找是否是符合。

- 如果不符合的下标值，就把当前值按位与计算找到槽，把槽中的值加一。

## 存在的问题

- 为什么槽的长度定义在2048？

- 2048个槽理论上来讲会存在碰撞问题，碰撞可能出现，出现后会误判。（可是没举出碰撞出现问题的例子来）

- 如何基于时间复杂度分析，来分析为什么这个算法运行快？

写的有错误的地方欢迎指出，也欢迎留言一起讨论。
