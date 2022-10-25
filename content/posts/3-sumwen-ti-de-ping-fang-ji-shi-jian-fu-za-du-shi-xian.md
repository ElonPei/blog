---
title: 3-SUM问题的平方级时间复杂度实现
date: 2017-12-05
tags:
  - Algorithm
---

本题为《算法4》作者 Robert Sedgewick 和 Kevin Wayne 在 Cursera 上开设的公开课的习题解答，本题出自以下地址中的课后题。

https://www.coursera.org/learn/algorithms-part1/quiz/lhs1X/interview-questions-analysis-of-algorithms-ungraded


### 原题


3-SUM in quadratic time. Design an algorithm for the 3-SUM problem that takes time proportional to n2 in the worst case. You may assume that you can sort the n integers in time proportional to n2 or better.


Note: these interview questions are ungraded and purely for your own enrichment. To get a hint, submit a solution.

<!--more-->


### 解题


```Java
public class ThreeSum {
    public static int threeSumProblem(int[] a) {
        int cnt = 0;
        for (int i = 0; i < a.length - 2; i++) {
            int j = i + 1;
            int k = a.length - 1;
            while (j < k) {
                int sum = a[i] + a[j] + a[k];
                if (sum == 0) {
                    cnt++;
                }
                if (sum >= 0) {
                    k--;
                } else {
                    j++;
                }
            }
        }
        return cnt;
    }
    public static void main(String[] args) {
        int[] a = {-40, -30, -20, -10, 0, 5, 10, 40};
        int cnt = threeSumProblem(a);
        StdOut.println(cnt);
    }
}
```
