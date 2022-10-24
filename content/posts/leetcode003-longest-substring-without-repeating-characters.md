---
title: LeetCode003 Longest Substring Without Repeating Characters
date: 2022-04-24
tags:
  - Algorithm
  - LeetCode
---

## 读题


求一个字符串的最长不重复子串


![Untitled](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/LeetCode003.png)


## 暴力解法一


🚫 官方提供的一种解题思路，这个方法在字符串特别长的时候会超时，官方原题直接运行也无法通过测试用例。


思路：


双循环遍历出所有可能的子字符串

判断子串是否是一个子串

进行最大值求解


`java
blic static int lengthOfLongestSubstring(String s) {
  int max = 0;
  for (int i = 0; i < s.length(); i++) {
      for (int j = i; j < s.length(); j++) {
          if (isSubstring(s, i, j)) {
              max = Math.max(max, j - i + 1);
          }
      }
  }
  return max;
ivate static boolean isSubstring(String s, int start, int end) {
  Map<Character, Integer> map = new HashMap<>();
  for (int i = start; i <= end; i++) {
      char c = s.charAt(i);
      map.put(c, map.get(c) == null ? 1 : map.get(c) + 1);
      if (map.get(c) > 1) {
          return false;
      }
  }
  return true;
```


## 暴力解法二


思路：


创建一个 `list` 用来存储所有的子串

遍历字符，把所有可能的子串添加到 `list` 中

然后遍历 `list` 求出最长的子串即可


`java
blic static int lengthOfLongestSubstring(String s) {
List<String> list = new ArrayList<>();
// 算出所有可能的子串
StringBuilder substring = new StringBuilder();
char[] chars = s.toCharArray();
for (int i = 0; i < chars.length; i++) {
    char c = chars[i];
    if (!substring.toString().contains(c + "")) {
        substring.append(c);
    } else {
        list.add(substring.toString());
        i = i - substring.length();
        substring = new StringBuilder();
    }
}
list.add(substring.toString());
// 求出最长子串
int max = 0;
for (String s1 : list) {
    int len = s1.length();
    if (len > max) {
        max = len;
    }
}
return max;
```


暴力算法时间复杂度和空间复杂度表现都比较差，我们接着思考新的思路。


## 窗口移动解法


思路：


创建一个 set 来存储窗口内容。

如果 set 中包含当前遍历窗口尾部字符，首部窗口后移。

否则尾部后移。

每当尾部后移后，保存当前最大子串长度。

循环结束后返回结果即可。


`java
blic int lengthOfLongestSubstring(String s) {
  int res = 0;
  int n = s.length();
  int first = 0;
  int last = 0;
  Set<Character> set = new HashSet<>();
  while (first < n && last < n){
      if (set.contains(s.charAt(last))){
          set.remove(s.charAt(first++));
      }else {
          set.add(s.charAt(last++));
          res = Math.max(res, last - first);
      }
  }
  return res;
```


测试用例返回情况：


![Untitled](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/LeetCode003 1.png)


The End!
