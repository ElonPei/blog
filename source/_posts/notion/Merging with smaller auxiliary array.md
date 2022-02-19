---
categories:
  - 算法与数据结构
created_time: February 19, 2022 9:30 AM
date: 2017/12/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Merging with smaller auxiliary array
---


本题为《算法4》作者 Robert Sedgewick 和 Kevin Wayne 在 Cursera 上开设的公开课的习题解答，本题为WEEK3中第一部分面试题。地址为： https://www.coursera.org/learn/algorithms-part1/quiz/LojjQ/interview-questions-mergesort-ungraded

### 原题

**Merging with smaller auxiliary array.** Suppose that the subarray 𝚊[𝟶] to 𝚊[𝚗−𝟷] is sorted and the subarray 𝚊[𝚗] to 𝚊[𝟸∗𝚗−𝟷] is sorted. How can you merge the two subarrays so that 𝚊[𝟶] to 𝚊[𝟸∗𝚗−𝟷] is sorted using an auxiliary array of length n (instead of 2n)?

### 读题

假定有一个数组长度为2n的数组，其中 `a[0] ~ a[n-1]` 与 `a[n] ~ a[2 * n - 1]` 分别有序，合并这两个切分的数组，并尝试至多使用长度为n的辅助数据进行`a[0] ~ a[2 * n -1]`排序。

### 解题

思考后，我的思路是这样的：

1. 首先判断前半部分的最后一个数是否小于后半部分的第一个数，如果是说明数组本来就是有序的，直接返回。
2. 其次判断前半部分的第一个数是否大于后半部分的最后一个数，如果是说明后半部分的所有数都小于前半部分，对前后对调即可排序。
3. 如果以上两种条件都不满足，对后半部分的第一个与前半部分从后向前进行一一比较，找到第一个小于的数，然后把前半部分遍历过的数据拷贝到辅助数组中。
4. 最后一步，对辅助数组和后半部分数据进行归并排序即可。

### Java实现

```
public class MergeTwoSortedArray {    private static int auxLength = 0;    public static void sort(Comparable[] a) {        int N = a.length / 2;        int _2N = a.length;        if (!less(a[N], a[N - 1])) {            return;        }        if (!less(a[0], a[_2N - 1])) {            for (int i = 0; i < N; i++) {                swap(a, i, N + i);            }            return;        }        Comparable[] aux = new Comparable[N];        for (int i = N - 1; i >= 0; i--) {            if (!less(a[N], a[i])) {                copyToAux(a, aux, i + 1, N);                mergeSort(a, aux, 0, N, i + 1);                return;            }        }    }    private static void copyToAux(Comparable[] a,                                  Comparable[] aux,                                  int start,                                  int end) {        for (int i = start, j = 0; i < end; i++) {            aux[j++] = a[i];            auxLength++;        }    }    private static void mergeSort(Comparable[] a,                                  Comparable[] aux,                                  int i,                                  int j,                                  int k) {        while (k < a.length) {            if (i > auxLength - 1) {                a[k++] = a[j++];            } else if (j > a.length - 1) {                a[k++] = aux[i++];            } else if (less(a[j], aux[i])) {                a[k++] = a[j++];            } else {                a[k++] = aux[i++];            }        }    }    public static void main(String[] args) {        Integer[] a = {10, 11, 12, 13, 5, 6, 7, 8};        MergeTwoSortedArray.sort(a);        for (Integer integer : a) {            StdOut.print(integer + " ");        }    }}
```