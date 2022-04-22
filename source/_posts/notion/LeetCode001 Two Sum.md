---
categories:
  - 算法与数据结构
created_time: February 19, 2022 9:30 AM
date: 2018/07/01
show_category: Yes
status: 待发布
tags:
  - LeetCode
updated_time: April 22, 2022 11:46 AM
title: LeetCode001 Two Sum
---


## 个人解题思路

双重for循环暴力解题，直接上代码。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int len = nums.length;
        for (int i = 0; i < len; i++) {
            for (int j = i + 1; j < len; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{};
    }
}
```

- 时间复杂度 O(n^2)
- 空间复杂度 O(1)

## 其他解题思路

个人的思路总是比较局限，需要学习其他思路来拓宽自己思考问题方式。

## 解法：Two-pass Hash Table

思路

- 我们先定义一个 HashTable，key 为数值中实际的值，value 为数组下标，并初始化
- 我们循环数组，用目标值对每个值进行减法运算
- 如果得到的结果在 HashTable 中，并且还不是它自身
- 结果就是当前值和目标值

上代码

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; i++) {
            int diff = target - nums[i];
            if (map.containsKey(diff) && map.get(diff) != i) {
                return new int[]{i, map.get(diff)};
            }
        }
        return null;
    }
}
```

- 时间复杂度 O(n)
- 空间复杂度 O(n)

The End！