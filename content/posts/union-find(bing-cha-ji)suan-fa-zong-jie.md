---
title: Union-find(并查集)算法总结
date: 2017-12-01
tags:
  - Algorithm
---

# 并查集算法总结


先挂偶像公开课的链接：[https://www.coursera.org/learn/algorithms-part1/home/welcome](https://www.coursera.org/learn/algorithms-part1/home/welcome)。


本文是自己算法理解的一个记录，表达的不够清楚勿喷，如果有收获，也可以留言点个赞。


## 算法概述


> 概述引用[维基百科](https://zh.wikipedia.org/wiki/%E5%B9%B6%E6%9F%A5%E9%9B%86)


在计算机科学中，并查集是一种树型的数据结构，用于处理一些不交集（Disjoint Sets）的合并及查询问题。有一个联合-查找算法（union-find algorithm）定义了两个用于此数据结构的操作：


Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。

Union：将两个子集合并成同一个集合。


<!-- more -->


## 解题一：自己写的暴力解题法，见笑了


暴力解题，只适合很小规模的数据量，数据量大就炸了。


数据结构：创建一个数组，每个数组中再创建一个数组，用来保存子集。

合并操作：找出两个子集的index，然后合并为一个。

查询两数联通：分别查询两个数归属的子集的index，判断index是否相同，相同便是联通。

查询归属子集：遍历查询归属那个子集。:(


### 主要代码


```java
private List<List<Integer>> components;
//初始化 - time complexity: N
for (int i = 0; i < N; i++) {
    List<Integer> component = new ArrayList<>();
    component.add(i);
    components.add(component);
}
//查找index - time complexity: N²
for (List<Integer> component : components) {
    for (Integer z : component) {
        if (p == z) {
            return components.indexOf(component);
        }
    }
}
//合并 - time complexity: N²+
int pidx = find(p);
int qidx = find(q);
if (pidx != qidx) {
    List<Integer> qcomponent = components.get(qidx);
    List<Integer> pcomponent = components.get(pidx);
    pcomponent.addAll(qcomponent);
    components.remove(qidx);
} 
```


## 解题二：快速查找


明白数据结构就一气呵成了，简单写了。此算法也不适合大规模数据使用，因为合并算法的时间复杂度为线性的。


数据结构：创建一个数组，合并就是把数组中的内容修改为相同的数，这样就可以判断是否是同一子集了。


### 部分代码


```java
//初始化 N
id = new int[N];
for (int i = 0; i < N; i++) {
    id[i] = i;
}
//查询 1
return id[p];
//合并 N(两个子集合并，需要循环修改数组中的内容)
int pid = find(p);
int qid = find(q);
for (int i = 0; i < id.length; i++) {
    if (id[i] == pid) {
        id[i] = qid;
    }
}
```


## 解题三：快速合并


该算法性能也差，时间复杂度都是线性的，但是数型结构后续优化潜力大。


数据结构，树型结构，合并时把被合并数指向合并数即可，也是只需要一个数组，用来保存该数指向那个节点。

合并操作：把被合并数的根节点指向合并数的根节点即可

查询归属子集：查询根节点

查询是否联通：查询两个数是否拥有同一个根节点


### 主要代码


```java
//初始化 同上 - N
//寻找根节点 N(取决于树的高度)
while (i != id[i]) {
    i = id[i];
}
return i;
//合并 - N(取决于树的高度)
if (!connected(p, q)) {
    int i = root(p);
    int j = root(q);
    id[i] = j;
}
//查询联通 - N(取决于树的高度)
return root(p) == root(q);
```


## 解题四：在快速合并算法的基础上进行优化


思考一下，快速合并为什么慢，主要是因为查找根节点的时候，树存在细长的情况，也就是高度高，造成高度高的主要原因是什么？或者说是什么操作造成的高度增长？对，是合并操作，所以需要思考合并的时候能有什么优化。


### 优化一：权重（也可以理解为树的大小）


合并时考虑权重，把权重低的树向权重高的树合并，理论上讲树的高度不会超过lgN（简单证明过程可以看开头提到的视频）。

需要一个额外的数组来保存节点权重。


#### 优化代码


```java
private int[] size;
//初始时全部初始化成1
size[i] = 1;
// 合并操作，重量小的向重量大的合并，建立一个数组来记录相应root节点的权重
// 巨大优化，合并查询操作时间复杂度提升到对数级别。
if (size[rootP] < size[rootQ]) {
    id[rootP] = rootQ;
    size[rootQ] += size[rootP];
} else {
    id[rootQ] = id[rootP];
    size[rootP] += size[rootQ];
}
```


### 优化二


我们可以进一步缩小树的高度，在计算根节点时，会顺序遍历子链中子节点，为了减少链的长度，把当前节点指向上上个节点，这样整棵树子节点更平。


### 代码


> 用偶像的话来说，one line code, amazing


```java
id[i] = id[id[i]];
```


## 总结


并查集算法是比较基础的算法，需要透彻的理解，数据结构并不复杂，算法思路也不复杂。
