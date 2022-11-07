---
title: LeetCode014 Longest Common Prefix
date: 2022-11-07
tags:
  - Algorithm
  - LeetCode
---


## 读题

原题 [链接](https://leetcode.com/problems/longest-common-prefix/)

![原题截图](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/2TkbKl.png)

给定一个字符串，求出字符串的最长公共前缀。

## 解题

从字符串的第一个字符开始比对，如果都相等那么就是公共前缀。

需要注意数组越界的问题。

代码

```java
public String longestCommonPrefix(String[] strs) {
    if (strs.length == 0) {
        return "";
    }
    StringBuilder lcp = new StringBuilder();
    int currentIdx = 0;
    while (true) {
        Character ch = null, prev = null;
        for (String str : strs) {
            char[] chars = str.toCharArray();
            if (chars.length == currentIdx) {
                currentIdx = -1;
                ch = null;
                break;
            }
            ch = chars[currentIdx];
            if (prev != null && prev != ch) {
                currentIdx = -1;
                ch = null;
                break;
            }
            prev = ch;
        }
        if (ch != null) {
            lcp.append(ch);
        }
        if (currentIdx == -1) {
            break;
        }
        currentIdx++;
    }
    return lcp.toString();
}
```

LeetCode 测试用例结果

![LeetCode 返回结果](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/4hHZ2L.png)
