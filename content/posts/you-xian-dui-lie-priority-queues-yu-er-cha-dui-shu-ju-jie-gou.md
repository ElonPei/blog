---
title: 优先队列( Priority Queues )与二叉堆数据结构
date: 2018-07-01
tags:
  - Algorithm
---


## 二叉堆的定义和性质


来自wiki: https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E5%A0%86


> 二叉堆是一种特殊的堆，二叉堆是完全二叉树或者是近似完全二叉树。

> 

> 二叉堆满足堆特性：父节点的键值总是保持固定的序关系于任何一个子节点的键值，且每个节点的左子树和右子树都是一个二叉堆。


<!-- more -->

## 优先队列定义


来自wiki: https://zh.wikipedia.org/wiki/%E5%84%AA%E5%85%88%E4%BD%87%E5%88%97


> 优先队列是计算机科学中的一类抽象数据类型。优先队列中的每个元素都有各自的优先级，优先级最高的元素最先得到服务；优先级相同的元素按照其在优先队列中的顺序得到服务。优先队列往往用堆来实现。

## 最大值二叉堆实现思路备忘


* 根节点是整棵树的最大的值。

* 假设某节点的index为`k`，它的父节点的index为`k/2`，他的两个子节点的index分别为`2k`和`2k + 1`。

* 二叉堆涉及到元素的下沉和上浮操作。

* 二叉堆的插入思路：先插入到树尾，然后对该元素做上浮操作。

* 二叉堆删除最大值并返回该元素思路：将根元素和尾元素替换，然后对根元素进行下沉操作，对尾元素进行置空操作。

## Java实现最大值优先队列


```Java
/**
 * 优先队列的实现
 * 数据结构：二叉堆
 *
 * @author elong
 * @version V1.0
 * @date 2017/12/25
 */
public class MaxPQ<Key extends Comparable<Key>> {

private Key[] pq;
private int N;

public MaxPQ(int capacity) {
    pq = (Key[]) new Comparable[capacity + 1];
}

public boolean isEmpty() {
    return N == 0;
}

/**
 * lgN
 */
public void insert(Key key) {
    if (N == pq.length - 1) {
        resize(pq.length * 2);
    }
    pq[++N] = key;
    swim(N);
}

/**
 * lgN
 */
public Key delMax() {
    if (isEmpty()) {
        throw new NoSuchElementException("Priority queue underflow");
    }
    Key max = pq[1];
    exch(1, N--);
    sink(1);
    pq[N + 1] = null;
    if (N > 0 && N == (pq.length - 1) / 4) {
        resize(pq.length / 2);
    }
    return max;
}

/**
 * 上浮
 */
public void swim(int k) {
    while (k > 1 && less(k / 2, k)) {
        exch(k / 2, k);
        k = k / 2;
    }
}

/**
 * 下沉
 */
public void sink(int k) {
    while (2 * k <= N) {
        int j = 2 * k;
        if (j < N && less(j, j + 1)) {
            j++;
        }
        if (!less(k, j)) {
            break;
        }
        exch(k, j);
        k = j;
    }
}

private void exch(int i, int j) {
    Key swap = pq[i];
    pq[j] = pq[i];
    pq[i] = swap;
}

private boolean less(int i, int j) {
    return pq[i].compareTo(pq[j]) < 0;
}

private void resize(int capacity) {
    assert capacity > N;
    Key[] temp = (Key[]) new Object[capacity];
    for (int i = 1; i <= N; i++) {
        temp[i] = pq[i];
    }
    pq = temp;
}

}
```


以上，二叉堆是树中最简单也是最基础的数据结构，有好的基础才会学懂后续相对复杂的数据结构。
