---
title: 双调数组( bitonic array )的高效( ~2lgN )搜索
date: 2018-01-01
tags:
  - Algorithm
---


本题为《算法4》作者 Robert Sedgewick 和 Kevin Wayne 在 Cursera 上开设的公开课的习题解答，本题为WEEK1中第二部分课后题。地址为：

https://www.coursera.org/learn/algorithms-part1/quiz/lhs1X/interview-questions-analysis-of-algorithms-ungraded

### 原题


**Search in a bitonic array.** An array is *bitonic* if it is comprised of an increasing sequence of integers followed immediately by a decreasing sequence of integers. Write a program that, given a bitonic array of n distinct integer values, determines whether a given integer is in the array.

Standard version: Use ∼3lgn compares in the worst case.

Signing bonus: Use ∼2lgn compares in the worst case (and prove that no algorithm can guarantee to perform fewer than ∼2lgn compares in the worst case).


<!--more-->

### 读题


给定一个双调数组（是一个先递增后递减的数组），其中有 n 个不同的整型值，确定给定的整数是否存在于数组。

### 解题

#### 基础版：在最坏的情况下时间复杂度~3lgN实现。


最坏的情况下，查找最大值使用 lgN，查找左边使用 lgN，查找右边使用 lgN，共3lgN。


```Java

import edu.princeton.cs.algs4.StdOut;

/**
 * @author elong
 * @version V1.0
 * @date 04/12/2017
 */
public class BitonicArray {

public static void main(String[] args) {
    int[] a = {1, 2, 3, 4, 5, 6, 7, 10, 29, 28, 27, 23, 22, 19, 17};
    int result = find(a, 6);
    StdOut.println(result);
}

public static int find(int[] a, int key) {
    int maxIdx = findTheMaxIdx(a);
    if (key == a[maxIdx]) {
        return maxIdx;
    }
    int first = findFirst(a, maxIdx, key);
    if (first != -1) {
        return first;
    }
    return findLast(a, maxIdx, key);
}

/**
 * Use the binary search find max value
 * ~lgN
 */
public static int findTheMaxIdx(int[] a) {
    int lo = 0;
    int hi = a.length - 1;
    while (lo < hi) {
        int mid = (lo + hi) / 2;
        if (a[mid] > a[mid + 1]) {
            hi = mid;
        } else {
            lo = mid + 1;
        }
    }
    return hi;
}

/**
 * Use the binary search find the increasing sequence
 * ~lgN
 */
private static int findFirst(int[] a, int maxIdx, int key) {
    int loIdx = 0;
    int hiIdx = maxIdx;
    while (loIdx <= hiIdx) {
        int mid = (loIdx + hiIdx) / 2;
        if (a[mid] > key) {
            hiIdx = mid - 1;
        } else if (a[mid] < key) {
            loIdx = mid + 1;
        } else {
            return mid;
        }
    }
    return -1;
}

/**
 * Use the binary search find the decreasing sequence
 * ~lgN
 */
private static int findLast(int[] a, int maxIdx, int key) {
    int loIdx = maxIdx;
    int hiIdx = a.length - 1;
    while (loIdx <= hiIdx) {
        int mid = (loIdx + hiIdx) / 2;
        if (a[mid] < key) {
            hiIdx = mid - 1;
        } else if (a[mid] > key) {
            loIdx = mid + 1;
        } else {
            return mid;
        }
    }
    return -1;
}
}

```

#### 进阶版：在最坏的情况下时间复杂度~2lgN 实现。


课程中提示，在把查询双调数组最高点去掉即可实现时间复杂度为~2lgN，自己没思路，在学习网上别人转载的这篇文章后豁然开朗，链接为：http://blog.csdn.net/fiveyears/article/details/11263381

##### 1) 对原文概述分析


> The key insight is that if you know a[lo] <= key < a[hi] (The open inequality for a[hi] is important!), then you can use a binary search on the semi-open range [lo,hi), even if a is bitonic.  


解决这道题的关键在于如果你知道 `a[lo] <= key < a[hi]` (不等于a[hi]是个重要因素)，如果这是个双调数组，你就可以用二分查找法对[lo,hi)进行查找。

##### 2) 对原文论证分析


> Proof: If a is bitonic in the range [lo,hi), then we can divide up the range into [lo, k), [k, peak), [peak, hi) where k is the first index with a[k] > a[hi].  We know the answer can only fall into the first region, [lo,k), since key < a[hi] < a[k], so everything in the second two regions are necessarily > key. 


如果你有一个在[lo,hi)范围内的双调数组，我们可以将其分为三个部分，分别为`[lo, k)` `[k, peak)` `[peak,hi)` ，其中 `a[k] > a[hi]` ，当`key < a[hi] < a[k]`   时，结果只能落入第一部分 `[lo,k)` ，所以任何数如果落入第二部分必须满足大于 `a[k]`  (注：原文有笔误)。

##### 3) 本题的解题思路


判断要查询的 `key` 是否小于 `a[mid]` ，如果小于，说明 `key` 不是落在左侧 `[lo, mid)` 或者右侧 `(mid, hi]` 的范围内，对左侧或者右侧进行二分查找即可，如果大于，判断 `mid` 处于左侧还是右侧，如果处于左侧 `lo = mid`，如果处于右侧 `hi = mid` 。


```Java

public class FastBitonicArraySearch {

private static boolean leftSearch(int[] a, int key, int lo, int hi) {
    while (lo < hi - 1) {
        int mid = (lo + hi) / 2;
        if (key < a[mid]) {
            hi = mid;
        } else {
            lo = mid;
        }
    }
    return key == a[lo];
}

private static boolean rightSearch(int[] a, int key, int lo, int hi) {
    while (lo < hi - 1) {
        int mid = (lo + hi) / 2;
        if (key < a[mid]) {
            lo = mid;
        } else {
            hi = mid;
        }
    }
    return key == a[hi];
}

public static boolean bitonicSearch(int[] a, int key) {
    int lo = 0, hi = a.length - 1;
    while (lo < hi - 1) {
        int mid = (lo + hi) / 2;
        if (key < a[mid]) {
            return leftSearch(a, key, lo, mid) || rightSearch(a, key, mid, hi);
        } else {
            if (a[mid] < a[mid + 1]) {
                lo = mid;
            } else {
                hi = mid;
            }
        }
    }
    return key == a[lo] || key == a[hi];
}

/**
 * Test OutPut:
 * 1 is in array? true
 * 2 is in array? true
 * 3 is in array? true
 * 4 is in array? true
 * 5 is in array? true
 * 6 is in array? true
 * 29 is in array? true
 * 28 is in array? true
 * 27 is in array? true
 * 23 is in array? true
 * 22 is in array? true
 * 19 is in array? true
 * 17 is in array? true
 * 16 is in array? true
 * 15 is in array? true
 * 14 is in array? true
 * 13 is in array? true
 * 12 is in array? true
 * 11 is in array? true
 * 10 is in array? true
 * 9 is in array? true
 * 8 is in array? true
 * 7 is in array? true
 * 1000 is in array? false
 * 0 is in array? false
 */
public static void main(String[] args) {
    int[] a = {1, 2, 3, 4, 5, 6, 29, 28, 27, 23, 22, 19, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7};
    for (int i : a) {
        boolean b = bitonicSearch(a, i);
        System.out.println(i + " is in array? " + b);
    }
    System.out.println("1000 is in array? " + bitonicSearch(a, 1000));
    System.out.println("0 is in array? " + bitonicSearch(a, 0));
}
}

```


以上，分析完毕，转载请注明出处。
