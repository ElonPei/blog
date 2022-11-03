---
title: LeetCode013 Roman To Integer
date: 2022-11-03
tags:
  - Algorithm
  - LeetCode
---


## 读题

原题 [链接](https://leetcode.com/problems/roman-to-integer/)

![lc013](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/NU1BhN.png)

与 [[LeetCode012 Integer to Roman]] 相反，把罗马数表示转换成一个数字。

## 解题

从左到右依次遍历转换，对转换的结果相加即可。

唯一的难点是对于 `IV` 这种反着表示的形式该如何处理？通过观察，反着表示的形式都是后一个大于前一个，因此我们可以判断如果当前的值大于前一个，只需要用后一个减去前一个即可得到当前需要相加的值。

对于反着的情况，还需要减去上次循环相加的值，否则会重复加一次得到错误的结果。

代码

```java
private static final Map<Character, Integer> map = new HashMap<Character, Integer>() {{
    put('V', 5);
    put('I', 1);
    put('X', 10);
    put('L', 50);
    put('C', 100);
    put('D', 500);
    put('M', 1000);
}};

public static int romanToInt(String s) {
    int sum = 0;
    int prev = 0;
    for (char ch : s.toCharArray()) {
        Integer current = map.get(ch);
        if (current <= prev) {
            sum += current;
        } else {
            sum += current - prev - prev;
        }
        prev = current;
    }
    return sum;
}
```

LeetCode 测试用例执行结果

![lc013 result](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/dCmSdi.png)
