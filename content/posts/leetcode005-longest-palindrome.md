---
title: LeetCode005 Longest Palindrome
date: 2022-04-28
---

![](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/Longest%20Palindrome.png)

## 读题

给定一个字符串，求字符串的最长回文串。

## 解题

回文串一定是对称的，我们每次循环都是从左右拓展。

我们求得每次循环得到的回文串的长度，求该长度有两种情况

1. 回文串长度是奇数，这个时候我们从中间的字符开始左右循环拓展。

2. 回文串长度是偶数，我们从最中间的两个数开始拓展。

取出最长的回文串，记录该回文串的开始节点和结束节点的索引。

根据索引切分字符串返回即可。

### 代码如下


```jsx
public static String longestPalindrome(String s) {
  int start = 0, end = 0;
  for (int i = 0; i < s.length(); i++) {
      int len = Math.max(expandAroundCenter(s, i, i), expandAroundCenter(s, i, i + 1));
      if (len > end - start) {
          start = i - (len - 1) / 2;
          end = i + len / 2;
      }
  }
  return s.substring(start, end + 1);
}

public static int expandAroundCenter(String s, int left, int right) {
  int l = left, r = right;
  while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
      l--;
      r++;
  }
  return r - l - 1;
}
```


时间复杂度：`O(n^2)`


空间复杂度：`O(1)`

## 测试用例结果

![](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/Longest%20Palindrome%201.png)

The End！
