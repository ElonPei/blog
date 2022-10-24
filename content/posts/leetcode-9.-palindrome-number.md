---
title: LeetCode 9. Palindrome Number
date: 2018-07-01
---


## 简要说明

判断一个数是否是回文数。

## 代码


`java
ass Solution {
public boolean isPalindrome(int x) {
    char[] chars = String.valueOf(x).toCharArray();
    int lo = 0;
    int hi = chars.length - 1;
    while (lo < hi) {
        if (chars[lo++] != chars[hi--]) {
            return false;
        }
    }
    return true;
}
```

## LeetCode 评分


![](https://raw.githubusercontent.com/peiel/oss/master/uPic/chENgd.png)
