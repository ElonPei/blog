---
title: 堆排序(HeapSort)实现
date: 2017-12-01
tags:
  - Algorithm
---

## 堆排序实现思路


堆排序是基于二叉堆数据结构实现的排序算法，排序思路非常简单，主要包含以下两步。


<!-- more -->


**第一步：对N个元素构建 max-heap 数据结构**


逆序对所有二叉子树的父级进行下沉操作，二叉子树的最大父级元素为`n/2`。  


**第二步：对最大元素替换移除**


把根元素和尾元素进行替换操作，然后长度计数器去掉尾元素的索引，并对根元素进行下沉操作。


## Java实现代码


```Java
public class HeapSort {

    public static void sort(Comparable[] pq) {
        int n = pq.length;
        for (int i = n / 2; i >= 1; i--) {
            sink(pq, i, n);
        }
        while (n > 1) {
            exch(pq, 1, n);
            sink(pq, 1, --n);
        }
    }

    /**
     * 堆元素下沉
     */
    private static void sink(Comparable[] pq, int k, int n) {
        while (2 * k <= n) {
            int j = 2 * k;
            if (j < n && less(pq, j, j + 1)) {
                j++;
            }
            if (!less(pq, k, j)) {
                break;
            }
            exch(pq, j, k);
            k = j;
        }
    }

    /**
     * 判断i元素是否小于j元素
     */
    private static boolean less(Comparable[] pq, int i, int j) {
        return pq[i - 1].compareTo(pq[j - 1]) < 0;
    }

    /**
     * 替换方法
     */
    private static void exch(Comparable[] pq, int i, int j) {
        Comparable swap = pq[i - 1];
        pq[i - 1] = pq[j - 1];
        pq[j - 1] = swap;
    }

    public static void main(String[] args) {
        Integer[] a = {4, 4, 4, 4, 4, 4, 4, 4, 3, 5, 6, 9, 19, 23, 54, 75, 1, 88, 44, 45, 54, 78, 12, 14, 15, 16, 11, 991, 765, 28, 29, 49, 81, 80};
        HeapSort.sort(a);
        for (Integer i : a) {
            StdOut.print(i + " ");
        }
    }

}
```


## 个人遇到的排序逻辑难点


1. 下沉操作的循环如何判断？答：判断子树是否小于等于树长

2. 下沉操作的两个子树索引的判断？答：如程序中判断。

3. less()方法为何报数组越界异常？答：因为二叉堆的根元素的索引是从1开始的，所以做数据操作时需要对索引减1。



以上堆排序完成，转载请注明出处。
