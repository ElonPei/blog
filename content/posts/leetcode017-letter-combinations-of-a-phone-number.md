---
title: LeetCode017 Letter Combinations of a Phone Number
date: 2022-11-16
tags:
  - Algorithm
  - LeetCode
---


## 读题

![problem](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/osTnju.png)

有个手机表盘，每个数字都有对应的 `n` 个字母。

给定一个数字的组合，返回数字对应的字母的所有组合的集合。

## 解题

本题有一定的难度，通过普通的循环嵌套很难有清晰的思路。

通过观察我们可以发现如下两个规律：

1. 我们把拼接的过程想想成一棵树，当前问题可以拆分为多个子问题，遍历当前数组并拼接到上一层。

2. 给定的数值如果是 `n` 位，返回的组合就是 `n` 位，符合递归的结束条件。

这个归类正好符合递归的使用场景，我们用递归来解该问题。

通过这篇文章 [[递归的理解]] 可以进一步理解递归。

代码如下：

```java
private static final Map<Character, List<String>> map = new HashMap<Character, List<String>>() {{
    put('2', Arrays.asList("a", "b", "c"));
    put('3', Arrays.asList("d", "e", "f"));
    put('4', Arrays.asList("g", "h", "i"));
    put('5', Arrays.asList("j", "k", "l"));
    put('6', Arrays.asList("m", "n", "o"));
    put('7', Arrays.asList("p", "q", "r", "s"));
    put('8', Arrays.asList("t", "u", "v"));
    put('9', Arrays.asList("w", "x", "y", "z"));
}};

// 使用递归来实现
public void loop(List<String> result, int idx, String curStr, char[] digitsChars) {
    // 递归的结束条件：目标字符和输入字符长度相同时
    if (curStr.length() == digitsChars.length) {
        result.add(curStr);
        return;
    }
    // 每次递归都会遍历当前索引下的字符，向下一层拼接
    for (String s : map.get(digitsChars[idx])) {
        loop(result, idx + 1, curStr + s, digitsChars);
    }
}

public List<String> letterCombinations(String digits) {
    List<String> result = new ArrayList<>();
    if (digits.equals("")) {
        return result;
    }
    loop(result, 0, "", digits.toCharArray());
    return result;
}
```

LeetCode 测试用例返回结果：

![result](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/dkhcb5.png)

时间复杂度：`O(4^n)`
