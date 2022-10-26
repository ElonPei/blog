---
title: LeetCode007 Reverse Integer
date: 2018-07-01
---

## 读题

给定一个32位的整型，反转数字部分并返回一个整型。

Ex1：

```
Input: 123
Output: 321
```

Ex2:

```
Input: -123
Output: -321
```

Ex3:

```
Input: 120
Output: 21
```

## 解题思路

### 1. 自己的思路

转换成一个字符串，然后反转，再加一步负数的判断就搞定了，简单。

代码

```java
  public int reverse(int x) {
      String xs = String.valueOf(x);
      char[] chars = xs.toCharArray();
      StringBuilder result = new StringBuilder();
      for (int i = chars.length - 1; i >= 0; i--) {
          if (i == 0 && chars[i] == '-') {
              result.insert(0, '-');
          } else {
              result.append(chars[i]);
          }
      }
      try {
          return Integer.parseInt(result.toString());
      } catch (NumberFormatException e) {
          return 0;
      }
  }
```

### 2. 官方思路

数学计算的思路，简单来讲，就是从个位依次 pop（弹）出数字，在累加到结果中即可。

代码

```java
    public static int reverse2(int x) {
        int result = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (result > Integer.MAX_VALUE / 10 || (result == Integer.MAX_VALUE / 10 && pop > 7)) {
                return 0;
            }
            if (result < Integer.MIN_VALUE / 10 || (result == Integer.MIN_VALUE / 10 && pop < -8)) {
                return 0;
            }
            result = result * 10 + pop;
        }
        return result;
    }
```

不能通过简单的捕获异常来判断，比如 `1534236469` 返回的结果就是不正确的。

需要特殊注意的是为什么在判断最大值和最小值的时候，还需要额外的判断 `pop > 7` 和 `pop < -8` 的情况？

> 自己一开始并没有想出来，这里引用leetcode上大佬的回答。

![http://nboss-002.oss-cn-qingdao.aliyuncs.com//uPic/n6WTQy.png](http://nboss-002.oss-cn-qingdao.aliyuncs.com//uPic/n6WTQy.png)

[原答案](https://leetcode.com/problems/reverse-integer/solution/)解释的很清楚了，不再赘述。
