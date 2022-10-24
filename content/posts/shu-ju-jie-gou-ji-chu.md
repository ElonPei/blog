---
title: 数据结构基础
date: 2021-10-31
tags:
  - Algorithm
  - Java
---

## 一、数据结构的存储方式

只有两种

- 数组

- 链表

## 二、数据结构的基本的操作

线性迭代结构

 ```java
 for(int i = 0; i < arr.length; i++){
   // ...
 }
```


链表的迭代框架，兼具迭代和递归结构


 ```java
 class ListNode {
   int val;
 ListNode next;
 }
 
 void traverse(ListNode head){
   for (ListNode p = head; p != null; p = p.next) {
           // 迭代访问
   }
 }
 
 void traverse(ListNode head){
       // 递归访问
       traverse(head.next);
 }
```


二叉树的遍历框架，典型的非线性递归遍历结构


 ```java
 class TreeNode{
   int val;
   TreeNode left, right;
 }
 
 void traverse(TreeNode root){
   traverse(root.left);
   traverse(root.right);
 }
```


二叉树遍历框架的拓展，可以拓展为N叉树的遍历框架


 ```java
 class TreeNode{
   int val;
   TreeNode left, right;
 }
 
 void traverse(TreeNode root){
   for (TreeNode child: root.children)
       traverse(child);
 }
```
