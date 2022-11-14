---
title: LeetCode016 Three Sum Closest
date: 2022-11-14
tags:
  - Algorithm
  - LeetCode
---


## 读题

![lc016](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/yIjPD9.png)

给定一个数组和目标整型值，求数组中任意三个数相加得到的结果最接近目标值。

## 解题

对数组依次遍历，遍历中定义两个索引，一个在最左侧，一个在最右侧，并向内移动。

左右两个索引在移动的过程中求出和，并判断是否接近目标值，如果更接近，赋值。

返回目标值。

代码如下，每一步都有详细的注释。

```java
public int threeSumClosest(int[] nums, int target) {
    // 初始化 result
    int result = nums[0] + nums[1] + nums[nums.length - 1];
    // 使 nums 有序
    Arrays.sort(nums);
    // 遍历 nums
    for (int i = 0; i < nums.length - 2; i++) {
        // 定义左边的索引和右边的索引
        int l = i + 1, r = nums.length - 1;
        // 让左边的索引和右边的索引向内遍历
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            // 如果 sum 比 target 大，那就 r 向前，反之 l 向后
            if (sum > target) {
                r--;
            } else {
                l++;
            }
            // 判断 sum 是不是更接近目标值，如果更接近，赋值即可
            if (Math.abs(target - sum) < Math.abs(target - result)) {
                result = sum;
            }
        }
    }
    return result;
}
```

LeetCode 测试用例返回结果

![结果](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/0I9oF0.png)
