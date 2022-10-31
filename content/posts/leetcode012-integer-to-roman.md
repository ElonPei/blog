---
title: LeetCode012 Integer to Roman
date: 2022-10-30
tags:
  - Algorithm
  - LeetCode
---


## 读题

原题 [链接](https://leetcode.com/problems/integer-to-roman/)

![原题](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/QHCscV.png)

给定一个整型的数字，转换成罗马数来表示。

## 解题

定义连个数组，由大到小分别依次存储所有符号和数字。

对每个数字进行整除取整，比如 `2100 / 1000 = 2` ，这种情况我们就需要循环两次得到 `MM`。

由 `1000` 开始遍历到 `1` 结束转换。

代码

```java
private static final int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
private static final String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

public static String intToRoman(int num) {
    if (num < 1 || num > 3999) {
        return "";
    }
    StringBuilder sb = new StringBuilder();
    int i = 0;
    while (num > 0) {
        int k = num / values[i];
        for (int j = 0; j < k; j++) {
            sb.append(symbols[i]);
            num -= values[i];
        }
        i++;
    }
    return sb.toString();
}
```

LeetCode 返回结果

![返回结果](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/kG1ikj.png)
