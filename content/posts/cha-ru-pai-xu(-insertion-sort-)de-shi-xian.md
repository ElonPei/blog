---
title: 插入排序( Insertion Sort )的实现
date: 2017-12-13
tags:
  - Algorithm
---

## 插入排序的排序逻辑

插入排序也是从左到右进行遍历，顾明思议，插入指的是对当前索引向前插入到合适的位置。

不停的对当前辅助索引与前一个进行比较，如果前一个大于当前辅助索引，进行替换，然后再次向前一步进行比较，如果前一个索引小于当前辅助索引，退出当前循序，然后主索引加一并进行下一轮循环。

插入排序的增长阶数为平方级。

## Java实现插入排序

```java
public class InsertionSort {

  public static void sort(Comparable[] a) {
      for (int i = 1; i < a.length; i++) {
          for (int j = i; j > 0; j--) {
              if (less(a[j], a[j - 1])) {
                  swap(a, j, j - 1);
              } else {
                  break;
              } 
          }
      }
  }

  public static void main(String[] args) {
      Integer[] a = {6, 5, 7, 8, 10, 1, 100, 23, 76, 19, 11};
      sort(a);
      for (Integer integer : a) {
          StdOut.print(integer + " ");
      }
  }

}

```

插入排序的实现也很简单，它是希尔排序的基础，执行效率相对于选择排序更高。
