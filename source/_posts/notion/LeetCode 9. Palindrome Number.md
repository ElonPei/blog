---
categories:
  - 算法与数据结构
created_time: February 19, 2022 9:30 AM
date: 2018/07/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: LeetCode 9. Palindrome Number
---


## 简要说明

判断一个数是否是回文数。

## 代码

```
class Solution {    public boolean isPalindrome(int x) {        char[] chars = String.valueOf(x).toCharArray();        int lo = 0;        int hi = chars.length - 1;        while (lo < hi) {            if (chars[lo++] != chars[hi--]) {                return false;            }        }        return true;    }}
```

## LeetCode 评分

![https://raw.githubusercontent.com/peiel/oss/master/uPic/chENgd.png](https://raw.githubusercontent.com/peiel/oss/master/uPic/chENgd.png)