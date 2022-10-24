---
title: 快速排序(QuickSort)的实现
date: 2017-12-01
---

## 快速的排序逻辑

> 快速排序的主要思想是递归

- 第一步：对需要排序的所有元素进行Shuffle。

- 第二步：取第一个元素为区分元素`k`，取第二个元素为`i`，取最后一个元素为`j`，`i`向右`j`向左分别迭代，最后替换`k`元素和`j`元素，最终保持`k`左侧的元素都小于`k`，右侧元素都大于`k`。

- 第三步：不停对左右部分再次进行上一步动作，最终排序完成。

## Java代码实现

 ```Java
 public class QuickSort {
 
 public static void sort(Comparable[] a) {
     Shuffle.shuffle(a);
     sort(a, 0, a.length - 1);
 }
 
 private static void sort(Comparable[] a, int lo, int hi) {
     if (hi <= lo) {
         return;
     }
     int j = partition(a, lo, hi);
     sort(a, lo, j - 1);
     sort(a, j + 1, hi);
 }
 
 private static int partition(Comparable[] a, int lo, int hi) {
     int i = lo, j = hi + 1;
     while (true) {
         while (less(a[++i], a[lo])) {
             if (i == hi) {
                 break;
             }
         }
         while (less(a[lo], a[--j])) {
             if (j == lo) {
                 break;
             }
         }
         if (i >= j) {
             break;
         }
         swap(a, i, j);
     }
     swap(a, lo, j);
     return j;
 }
 
 }
```

## 小优化

在数组大小较小时，使用插入排序比快速排序执行效率要高。

 ```
 private static final int INSERTIONSORT_THRESHOLD = 7;
```

 ```Java
 if ((hi - lo) < INSERTIONSORT_THRESHOLD) {
 	InsertionSort.sort(a, lo, hi);
 }
```

以上，快速排序的思想真的有些烧脑，实现起来坑还是很多，需要仔细仔细再仔细的思考每一个细节。
