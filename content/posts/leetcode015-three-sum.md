---
title: LeetCode015 Three Sum
date: 2022-11-08
tags:
  - Algorithm
  - LeetCode
---


## 读题

![读题](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/zaAjYo.png)

给定一个整型数组，返回所有不重复的三个值相加等于0的集合。

## 解题

### 方法一：暴力法

对所有的可能进行遍历，然后相加如果等于0就加入集合中。

在加入集合的前边要判断是否已经存在(也可以使用 `Set`)

代码

```java
public List<List<Integer>> threeSumBruteForce(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < nums.length; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        for (int j = i + 1; j < nums.length; j++) {
            if (j > i + 1 && nums[j] == nums[j - 1]) {
                continue;
            }
            for (int k = j + 1; k < nums.length; k++) {
                if (k > j + 1 && nums[k] == nums[k - 1]) {
                    continue;
                }
                if (nums[i] + nums[j] + nums[k] == 0) {
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[j]);
                    list.add(nums[k]);
                    res.add(list);
                }
            }
        }
    }
    return res;
}
```

时间复杂度 O^3

无法通过 `LeetCode` 测试用例

### 方法二：双节点遍历

需要对输入的数组进行排序

在暴力法的前提下做的优化

把暴力法中第二次循环和第三次循环总和 `O^2` 复杂度降低到 `O^1`

在左右两端分别放置两个索引，在求和比较小的时候，左端++，反之右端--。

相等的情况下左右都进行向内移动。

对于保证结果不是重复是本思路的一个难点，解决思路就是在等于0的情况后，再次遍历两个索引，如果下一个与当前相等，移动索引。

代码

```java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    System.out.println(Arrays.toString(nums));
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < nums.length - 2; i++) {
        int left = i + 1, right = nums.length - 1;
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            if (sum > 0) {
                right--;
            } else if (sum < 0) {
                left++;
            } else {
                List<Integer> list = new ArrayList<>(Arrays.asList(nums[i], nums[left], nums[right]));
                res.add(list);
                left++;
                right--;
                while (left < right && nums[left] == nums[left - 1]) {
                    left++;
                }
                while (left < right && right < nums.length - 2 && nums[right] == nums[right + 1]) {
                    right--;
                }
            }
        }
    }
    return res;
}
```

时间复杂度 `O^2`

LeetCode 测试用例情况

![结果](https://cdn.jsdelivr.net/gh/snail-tech/oss@master/uPic/Z1qXpt.png)
