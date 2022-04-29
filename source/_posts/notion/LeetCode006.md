---
categories:
  - 算法与数据结构
created_time: April 29, 2022 10:36 AM
date: 2022/04/29
excerpt: 字符串Z子形排列
show_category: Yes
status: 待发布
tags:
  - Algorithm
  - LeetCode
updated_time: April 29, 2022 10:58 AM
title: LeetCode006
---

Zigzag


![](/notion_images/bd8800c1445b6a4cb7d14da81ec41dee.png)

## 读题

把一个字符串Z字形（向左翻转）排列，然后输出排列后的正向顺序。

## 解题

<aside>

<img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 本题思路比较简单直观，我们简要说明下直接上代码。
</aside>

- 遍历字符串，按Z字形顺序输出到数组中。
- 我们定义一个方向，来代表向下遍历还是向上遍历。
- 循环结束后即可输出我们想要的结构。

### 代码

```jsx
public static String convert(String s, int numRows) {
    StringBuilder[] arr = new StringBuilder[numRows];
    int currentIdx = 0;
    boolean direction = true;
    int firstLineIdx = 0;
    int lastLineIdx = numRows - 1;
    for (char c : s.toCharArray()) {
        if (arr[currentIdx] == null) {
            arr[currentIdx] = new StringBuilder();
        }
        arr[currentIdx].append(c);
        if (direction && currentIdx < lastLineIdx) {
            currentIdx++;
        }
        if (!direction && currentIdx > firstLineIdx) {
            currentIdx--;
        }
        if (currentIdx == firstLineIdx || currentIdx == lastLineIdx) {
            direction = !direction;
        }
    }
    StringBuilder ans = new StringBuilder();
    for (StringBuilder sb : arr) {
        if (sb != null) {
            ans.append(sb);
        }
    }
    return ans.toString();
}
```

时间复杂度：`O(n)`

空间复杂度：`O(n)`

## 测试用例情况

![](/notion_images/fb55deacd38f120c0fe334ba24709802.png)

The End！