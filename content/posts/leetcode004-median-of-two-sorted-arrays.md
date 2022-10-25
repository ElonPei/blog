---
title: LeetCode004 Median of Two Sorted Arrays
date: 2022-04-27
tags:
  - Algorithm
  - LeetCode
---

Median of Two Sorted Arrays



![](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/LeetCode004.png)


## 读题


给定两个有序的数组，求出两个数组合并为一个有序数组，并且求出中位数。要求时间复杂度至少为 `O(log (m+n))` 。

## 解题

### 思路一：暴力破解法


在不考虑时间复杂度的情况下，我们使用暴力法解这个题的思路就非常简单，循环两个数组，对取出的值进行大小比对，小的往新的数组中放，然后对原数据索引加一，合并后对新的数组求中位数即可。


代码：


由于过于简单，忽略该代码。

### 思路二：二分查找


<aside>


<img class="emoji" draggable="false" alt="💁‍♂️" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f481-200d-2642-fe0f.png"/> 根据题目中时间复杂度的要求，对于这种对数级别的时间复杂度，我们能想到的第一个算法就是二分查找。

</aside>


该题难度比较大，我能想到二分查找，但是靠自己想不出思路，于是上网上查找思路，贴一个大佬的视频分析的[链接](https://www.youtube.com/watch?v=LPFhl65R7ww&t=13s&ab_channel=TusharRoy-CodingMadeSimple)，看完视频后，脑子中的思路就比较清晰了。


代码：


```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
  if (nums1.length > nums2.length) {
      return findMedianSortedArrays(nums2, nums1);
  }
  int x = nums1.length;
  int y = nums2.length;
  int low = 0, high = x;
  while (low <= high) {
      int partitionX = (low + high) / 2;
      int partitionY = (x + y + 1) / 2 - partitionX;
      int maxLeftX = partitionX == 0 ? Integer.MIN_VALUE : nums1[partitionX - 1];
      int minRightX = partitionX == x ? Integer.MAX_VALUE : nums1[partitionX];
      int maxLeftY = partitionY == 0 ? Integer.MIN_VALUE : nums2[partitionY - 1];
      int minRightY = partitionY == y ? Integer.MAX_VALUE : nums2[partitionY];
      if (maxLeftX <= minRightY && maxLeftY <= minRightX) {
          if ((x + y) % 2 == 0) {
              return (Math.max(maxLeftX, maxLeftY) + Math.min(minRightX, minRightY)) / 2d;
          } else {
              return Math.max(maxLeftX, maxLeftY);
          }
      } else if (maxLeftX > minRightY) {
          high = partitionX - 1;
      } else {
          low = partitionX + 1;
      }
  }
  throw new IllegalArgumentException("not found");
}
```


Test Case Result：


![](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/LeetCode004%201.png)


The End！
