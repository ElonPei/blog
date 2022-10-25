---
title: 希尔排序(ShellSort)的实现
date: 2017-12-01
tags:
  - Algorithm
---

## 希尔排序的排序逻辑

希尔排序在插入排序的基础上有了增量序列(increment sequence)的概念。

最优的增量序列是希尔排序性能的关键。

在增量 3*x* + 1 的情况下，最坏情况下比较次数为 $O(N^\frac{3}{2})$ 。

## Java实现希尔排序

```java
public class ShellSort {

    public static void sort(Comparable[] a) {
        int N = a.length;
        int h = 1;
        for (; h < N / 3; ) {
            h = 3 * h + 1;
        }
        for (; h >= 1; ) {
            for (int i = h; i < N; i++) {
                for (int j = i; j - h >= 0; j -= h) {
                    if (less(a[j], a[j - h])) {
                        swap(a, j, j - h);
                    } else {
                        break;
                    }
                }
            }
            h = h / 3;
        }
    }

    public static void main(String[] args) {
        Integer[] a = {6, 5, 7, 8, 10, 1, 100, 23, 76, 19, 11, 22, 18, 90, 21, 87, 78, 91, 68, 56, 57, 53, 38};
        sort(a);
        for (Integer integer : a) {
            StdOut.print(integer + " ");
        }
    }
    
}
```

有一点需要注意的就是，在判断前一个值小于当前值时，没必要再循环下去了，退出当前循环。

以上，希尔排序，完毕。
