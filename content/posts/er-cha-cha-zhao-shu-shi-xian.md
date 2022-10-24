---
title: 二叉查找树实现
date: 2017-12-01
---

今天是农历腊月二十四，也是过年前上班的最后一天，乘着工作不忙的功夫，把算法4公开课的第四周做个结束，本文总结第四周中第二节符号表(Symbol Tables)中的二叉查找树(Binary Search Tree)数据结构。



## 二叉查找树的性质


from : [wiki](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%85%83%E6%90%9C%E5%B0%8B%E6%A8%B9)


> 二叉查找树（英语：Binary Search Tree），也称二叉搜索树、有序二叉树（英语：ordered binary tree），排序二叉树（英语：sorted binary tree），是指一棵空树或者具有下列性质的二叉树：


> 若任意节点的左子树不空，则左子树上所有节点的值均小于它的根节点的值；

若任意节点的右子树不空，则右子树上所有节点的值均大于它的根节点的值；

任意节点的左、右子树也分别为二叉查找树；

没有键值相等的节点。


<!-- more -->


## 代码实现



`Java
port edu.princeton.cs.algs4.Queue;
*
 二叉查找树的实现

 @author elong
 @version V1.0
 @date 2018/1/23
/
blic class BST<Key extends Comparable<Key>, Value> {
  private Node root;
  private class Node {
      private final Key key;
      private Value value;
      private Node left, right;
      private int count;
      public Node(Key key, Value value) {
          this.key = key;
          this.value = value;
          this.count = 1;
      }
      @Override
      public String toString() {
          return key + " : " + value;
      }
  }
  public void put(Key key, Value value) {
      root = put(root, key, value);
  }
  private Node put(Node x, Key key, Value value) {
      if (x == null) {
          return new Node(key, value);
      }
      int cmp = key.compareTo(x.key);
      if (cmp < 0) {
          x.left = put(x.left, key, value);
      }
      if (cmp > 0) {
          x.right = put(x.right, key, value);
      }
      if (cmp == 0) {
          x.value = value;
      }
      x.count = 1 + size(x.left) + size(x.right);
      return x;
  }
  public Value get(Key key) {
      Node x = root;
      while (x != null) {
          int cmp = key.compareTo(x.key);
          if (cmp < 0) {
              x = x.left;
          } else if (cmp > 0) {
              x = x.right;
          } else {
              return x.value;
          }
      }
      return null;
  }
  public int size() {
      return size(root);
  }
  private int size(Node node) {
      if (node == null) {
          return 0;
      }
      return node.count;
  }
  public Key minKey() {
      Node node = minKey(root);
      if (node == null) return null;
      return node.key;
  }
  private Node minKey(Node node) {
      if (node.left == null) {
          return node;
      }
      return minKey(node.left);
  }
  public Key maxKey() {
      Node node = maxKey(root);
      if (node == null) return null;
      return node.key;
  }
  private Node maxKey(Node node) {
      if (node.right == null) {
          return node;
      }
      return maxKey(node.right);
  }
  /**
   * 小于等于给定Key的最大值
   */
  public Key floor(Key key) {
      Node node = floor(root, key);
      if (node == null) return null;
      return node.key;
  }
  private Node floor(Node node, Key key) {
      if (node == null) return null;
      int cmp = key.compareTo(node.key);
      if (cmp == 0) {
          return node;
      }
      if (cmp < 0) {
          return floor(node.left, key);
      }
      Node x = floor(node.right, key);
      if (x != null) {
          return x;
      }
      return node;
  }
  /**
   * 大于等于给定Key的最小值
   */
  public Key ceiling(Key key) {
      Node node = ceiling(root, key);
      if (node == null) {
          return null;
      }
      return node.key;
  }
  private Node ceiling(Node node, Key key) {
      if (node == null) return null;
      int cmp = key.compareTo(node.key);
      if (cmp == 0) {
          return node;
      }
      if (cmp > 0) {
          return ceiling(node.right, key);
      }
      Node x = ceiling(node.left, key);
      if (x != null) {
          return x;
      }
      return node;
  }
  /**
   * 小于Key的Node的数量
   */
  public int rank(Key key) {
      return rank(root, key);
  }
  private int rank(Node node, Key key) {
      if (node == null) {
          return 0;
      }
      int cmp = key.compareTo(node.key);
      if (cmp == 0) {
          return size(node.left);
      }
      if (cmp > 0) {
          return 1 + size(node.left) + rank(node.right, key);
      }
      return rank(node.left, key);
  }
  public Iterable<Key> keys() {
      Queue<Key> q = new Queue<>();
      inorder(root, q);
      return q;
  }
  private void inorder(Node node, Queue<Key> q) {
      if (node == null) {
          return;
      }
      inorder(node.left, q);
      q.enqueue(node.key);
      inorder(node.right, q);
  }
  public void delete(Key key) {
      delete(root, key);
  }
  private Node delete(Node node, Key key) {
      if (node == null) {
          return null;
      }
      int cmp = key.compareTo(node.key);
      if (cmp < 0) {
          node.left = delete(node.left, key);
      } else if (cmp > 0) {
          node.right = delete(node.right, key);
      } else {
          if (node.right == null) return node.left;
          if (node.left == null) return node.right;
          Node t = node;
          node = minKey(t.right);
          node.right = delMin(t.right);
          node.left = t.left;
      }
      node.count = 1 + size(node.left) + size(node.right);
      return node;
  }
  public void delMin() {
      delMin(root);
  }
  private Node delMin(Node node) {
      if (node.left == null) {
          return node.right;
      }
      node.left = delMin(node.left);
      node.count = 1 + size(node.left) + size(node.right);
      return node;
  }
  public static void main(String[] args) {
      BST<String, String> bst = new BST<>();
      bst.put("b", "b");
      bst.put("c", "c");
      bst.put("d", "d");
      bst.put("e", "e");
      bst.put("u", "r");
      bst.put("f", "r");
      bst.put("y", "r");
      bst.put("v", "r");
      bst.put("v", "r");
      bst.put("v", "r");
      bst.put("a", "r");
      System.out.println(bst.ceiling("a"));
      System.out.println(bst.maxKey());
      System.out.println(bst.size());
      System.out.println(bst.rank("z"));
      bst.delMin();
      for (String s : bst.keys()) {
          System.out.println(s);
      }
  }
```


## 本人思考

1. 递归思想的运用有待提高

2. 在写代码前，大脑中一定要有清晰的数据结构，否者容易写到一半思路断了。



The end
