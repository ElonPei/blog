---
title: 归并排序(MergeSort)的实现
date: 2017-12-01
tags:
  - Algorithm
---

## 归并排序的排序逻辑

* 第一步：新建一个原数组的副本数组。

* 第二步：将副本数组一分为二，左侧和右侧分别有序。

* 第三步：分别将两个数组从左到右比较，那个小把那个赋值到原数组。


注：递归调用排序方法，直到一分为二的两个数组只有一个元素。


<!-- more -->


## 归并排序的Java实现


```Java
public class MergeSort {

    public static void sort(Comparable[] a) {
        Comparable[] aux = new Comparable[a.length];
        sort(a, aux, 0, a.length - 1);
    }

    private static void sort(Comparable[] a, Comparable[] aux, int lo, int hi) {
        if (lo >= hi) { //注意判断，否者会造成无限递归栈溢出
            return;
        }
        int mid = (lo + hi) / 2;
        sort(a, aux, lo, mid);
        sort(a, aux, mid + 1, hi);
        merge(a, aux, lo, mid, hi);
    }

    private static void merge(Comparable[] a, Comparable[] aux, int lo, int mid, int hi) {
        int i = lo, j = mid + 1;
        for (int k = lo; k <= hi; k++) {
            aux[k] = a[k];
        }
        for (int k = lo; k <= hi; k++) {
            if (j > hi) {
                a[k] = aux[i++];
            } else if (i > mid || less(aux[j], aux[i])) {
                a[k] = aux[j++];
            } else {
                a[k] = aux[i++];
            }
        }
    }

    public static void main(String[] args) {
        Integer[] a = {4, 3, 5, 6, 9, 19, 23, 54, 75, 1, 88, 44, 45, 54, 78, 12, 14, 15, 16, 11, 991, 765, 28, 29, 49, 81, 80};
        MergeSort.sort(a);
        for (Integer i : a) {
            StdOut.print(i + " ");
        }
    }

}
```


## 性能优化一

在合并方法中，如果前一半的最大值小于后一半的最小值，那么这个数组本来就是有序的，直接返回。


```Java
if (less(a[mid], a[mid + 1])) {
    return;
}
```


本测试用例用，优化前合并方法执行`26`次，优化后合并方法执行`14`次，性能大大提高。


## 性能优化二

在数组大小较小时，使用插入排序比并归排序执行效率要高。查看了下JDK的`Arrays.java`并归排序的实现，采用和JDK的阀值一样。


```
private static final int INSERTIONSORT_THRESHOLD = 7;
```


```Java
if ((hi - lo) < INSERTIONSORT_THRESHOLD) {
    InsertionSort.sort(a, lo, hi);
}
```


以上，归并排序，转载请注明出处。
