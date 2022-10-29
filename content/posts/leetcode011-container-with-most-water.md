---
title: LeetCode011 Container With Most Water
date: 2022-10-29
tags:
  - Algorithm
  - LeetCode
---


![原题描述](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/wh7S7t.png)

## 读题

给定一个数组，每个数组都代表一个一定高度的柱子，任意选定两个柱子，往里边倒水，求最大面积。

## 解题思路

### 思路一：暴力法

循环两次，枚举出所有的组合，取最大值即可。

```java
public static int maxArea(int[] height) {
    int max = 0;
    int n = height.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int minLines = Math.min(height[i], height[j]);
            max = Math.max(max, minLines * (j - i));
        }
    }
    return max;
}
```

时间复杂度O(n^2)

空间复杂度O(1)

暴力法时间复杂度为平方级，无法通过测试用例。

![失败用例](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/MtcpdQ.png)

### 思路二：双指针法

左右两侧分别放置两个指针，那个短就向里移动一次，移动后比较取最大值即可。

```java
public static int maxArea(int[] height) {
    int max = 0;
    int left = 0, right = height.length - 1;
    while (left < right) {
        int area = (right - left) * Math.min(height[left], height[right]);
        max = Math.max(max, area);
        if (height[left] <= height[right]) {
            left++;
        } else {
            right--;
        }
    }
    return max;
}
```

时间复杂度O(n)

空间复杂度O(1)

![返回结果](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/IDbgUi.png)
