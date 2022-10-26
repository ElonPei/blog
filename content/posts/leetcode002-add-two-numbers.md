---
title: LeetCode002 Add Two Numbers
date: 2022-04-23
---

![](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/Add%20Two%20Number.png)

## 读题

有两个链表，链表的每个节点都保存一个数字，联表的头部为个位，对两个链表进行相加，得出一个新的链表，这种方式相较于直接 `int` 类型相加，没有最大位数的的限制，理论上可以相加无限位的数字。

## 解题

思路

- 定义一个头结点，用来表示结果的链表。

- 定义一个结果节点的引用，用来存储头结点的索引。

- 定义一个整型，用来存储相加大于10后需要进位的情况。

- 定义循环，当读取完 `l1` 和 `l2` 并且`进位`为0的时候跳出循环。

- 循环中操作

	- 执行相加

- 保存进位值

- 将结果保存到结果链表上

- l1 和 l2 跳转到下一个节点

## 上代码

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
ListNode node = new ListNode(0);
ListNode result = node;
int idx = 0;
while (l1 != null || l2 != null || idx != 0){
    int sum = (l1 != null ? l1.val : 0) + (l2 != null ? l2.val : 0) + idx;
    idx = sum / 10;
    node.next = new ListNode(sum % 10);
    node = node.next;
    l1 = l1 != null ? l1.next : null;
    l2 = l2 != null ? l2.next : null;
}
return result.next;
}
```

## LeetCode Test Cases Result

![](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/Add%20Two%20Number%20Result.png)

The End !
