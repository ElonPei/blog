---
title: 每日算法 #955 表达式计算树
date: 955
tags:
  - 2021-08-04
  - Algorithm
---

## 说明

给定一棵树，如下，根据树的子节点计算结果。

 ```
   *
  / \
 +    +
  / \  / \
 3  2  4  5
```

例子中的树应该返回结果 (3 + 2) * (4 + 5) = `45`

## 思路

采用分治法可以解决此问题，我们观察树，发现，任何一个树，都可以分解为一个子问题，最小子问题就是一个只有左右两个节点的树，思路如下。

 ```
 solove
   if 是一个运算符？
       左边 = solove(左节点)
       右边 = solove(右节点)
       返回 -> 计算(左边, 右边, 运算符)
   else
       返回 -> 当前节点值
```

## 代码

 ```java
 public static int solove(Node node) throws Exception {
   if (isOperator(node)) {
       return opration(solove(node.getLeft()), solove(node.getRight()), node.getItem());
   }
   return Integer.parseInt(node.getItem());
 }
 
 public static int opration(int left, int right, String item) throws Exception {
   if (item.equals("+")) {
       return left + right;
   }
   if (item.equals("-")) {
       return left - right;
   }
   if (item.equals("*")) {
       return left * right;
   }
   if (item.equals("/")) {
       return left / right;
   }
   throw new Exception("");
 }
 
 public static boolean isOperator(Node node) {
   return node.getItem().equals("+")
           || node.getItem().equals("-")
           || node.getItem().equals("*")
           || node.getItem().equals("/");
 }
```
