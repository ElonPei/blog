---
categories:
  - 算法与数据结构
created_time: February 19, 2022 9:16 AM
date: 2017/12/13
excerpt: 从左到右开始遍历，查询索引右侧的最小值，并与当前索引进行替换，这样就可以保持索引的左侧一直是有序的。
show_category: Yes
status: 待发布
tags:
  - Algorithm
updated_time: February 19, 2022 10:41 AM
title: 选择排序(SelectionSort)的实现
---


## 选择排序的排序逻辑

- 从左到右开始遍历，查询索引右侧的最小值，并与当前索引进行替换，这样就可以保持索引的左侧一直是有序的。
- 选择排序的增长阶数为平方级。

## Java实现该算法

```java
public class SelectionSort {

    private static void sort(Comparable[] a) {
        for (int i = 0; i < a.length; i++) {
            int min = i;
            for (int j = i + 1; j < a.length; j++) {
                if (less(a[j], a[min])) {
                    min = j;
                }
            }
            swap(a, i, min);
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

以上，很基础的算法，无需多说，转载请注明出处！