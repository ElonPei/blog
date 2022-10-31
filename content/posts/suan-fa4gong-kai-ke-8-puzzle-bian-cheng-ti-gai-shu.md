---
title: 算法4公开课 8-Puzzle 编程题概述
date: 2018-01-18
tags:
  - Algorithm
---


原题地址：http://coursera.cs.princeton.edu/algs4/assignments/8puzzle.html


本文主要针对原题做一个简单的概述，目的是理解8-Puzzle问题，以便方便自己日后查看。有看到这篇文章的小伙伴也可以发邮件一起交流。英文阅读能力强请忽略本文直接去原文。

## The problem


`8-Puzzle`问题是Noyes Palmer Chapman在1870年发明和推广的一个难题，在一个`3*3`的网络格子上放着`1~8`和一个空白位，需要实现的目标就是使用最少的步数，对`1~8`进行排序并使空白位在最后一格中，只允许将数字块横向和纵向移入空白块，以下为图例：


<!-- more -->


![8-Puzzle problem](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/006tNc79ly1fnkssf10hjj30ww05mgm3.jpg)

## 最佳优先搜索(Best-first search)


我们首先描述一个解决问题的通用的人工智能的方法**A*搜索算法**，我们定义一个游戏的搜索节点Board，该节点板为达到棋盘所做的移动次数，以及前任搜索节点。首先，插入一个初始化的搜索节点（该初始板，0 moves，前任搜索节点为 null）进入优先队列。然后，从优先级队列中删除具有最低优先级的搜索节点，并且将所有相邻搜索节点（从出列的搜索节点一次可达的节点）插入到优先队列中，重复这个过程知道出列的搜索节点与目标板一致。这种方法的成功取决于对搜索节点的优先级函数选择。我们认为有以下两个优先级函数：

*Hamming priority function.* 具有错误位置的块的数量加上到目前为止移动到达搜索节点的数量。直观的讲，具有错误位置上的少量块的搜索节点更接近目标，并且我们优选使用少量移动已经到达的搜索节点。

*Manhattan priority function.* 所有块到目标块的曼哈顿距离（垂直和水平的距离之和）的和加上到目前为止移动到达搜索节点的数量。


举个例子：


![8_Puzzle_img2.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/8_Puzzle_img2.jpg)


我们做一个关键的观察：解决 Puzzle 从给定一个优先级队列的搜索节点开始，我们需要做的移动的总次数（包含已经做出的动作）至少是其优先级，使用汉明或哈曼顿优先级函数。（对于汉明而言，每个一个至少需要移动一次到达它的目标位置。对于哈曼顿而言，每一个至少移动哈曼顿距离来到达它的目标位置。在计算汉明和哈曼顿的优先次序时，不要计算空白的方格。）因此，当目标板出队时，我们不仅发现从初始板到目标板的一系列移动，而且还发现了最少的移动次数。

## 一个关键的优化


最佳优先搜索有一个非常操蛋的特性，搜索节点对应的板块入队到优先队列多次，为了减少对不必要的搜索节点的探索，当我们对相邻的搜索节点入队时，不要对相邻节点和前任节点相同的board 进行入队操作。如下图：


![8_Puzzle_img3.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/8_Puzzle_img3.jpg)

## 第二个优化


为了避免每次在各种优先级队列构建时重新计算哈曼顿优先级，在构建搜索节点时预先计算其值，将其保存在常量中，在需要的时候返回。这种缓存技术是广泛适用的：考虑在多次计算相同数量的情况下使用它，并且计算该数量时瓶颈操作。

## 博弈树（Game Tree）


查看计算的一种方法是作为博弈树，其中每个搜索节点时博弈树中的节点，并且节点的子节点对应于其相邻搜索节点。博弈树的根节点是初始节点，内部节点已经被处理，叶节点保持在优先级队列中，在每一步中，A*搜索算法从优先级队列中删除具有最小优先级的节点并对其进行处理（通过将其子节点添加到博弈树和优先级队列中）。


![8_Puzzle_img4.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/8_Puzzle_img4.jpg)

## 检测不能解决的Puzzles


不是所有的初始 borad 通过合法的移动次数，包含以下：


![8_Puzzle_img5.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/8_Puzzle_img5.jpg)


这是一个非常重要的性质：8-puzzle问题中凡是无解的图，将除了空白栏之外的任意两个格子交换，都能得到可解图。而可解图通过一次交换得到的是无解图。

证明不是看的很懂，有兴趣的小伙伴可以去这个网站看看：http://www.cs.bham.ac.uk/~mdr/teaching/modules04/java2/TilesSolvability.html

## Board and Solver API


```
public class Board {
public Board(int[][] blocks)           // construct a board from an n-by-n array of blocks
                                     // (where blocks[i][j] = block in row i, column j)
public int dimension()                 // board dimension n
public int hamming()                   // number of blocks out of place
public int manhattan()                 // sum of Manhattan distances between blocks and goal
public boolean isGoal()                // is this board the goal board?
public Board twin()                    // a board that is obtained by exchanging any pair of blocks
public boolean equals(Object y)        // does this board equal y?
public Iterable<Board> neighbors()     // all neighboring boards
public String toString()               // string representation of this board (in the output format specified below)

public static void main(String[] args) // unit tests (not graded)
}
```


```
public class Solver {
public Solver(Board initial)           // find a solution to the initial board (using the A* algorithm)
public boolean isSolvable()            // is the initial board solvable?
public int moves()                     // min number of moves to solve initial board; -1 if unsolvable
public Iterable<Board> solution()      // sequence of boards in a shortest solution; null if unsolvable
public static void main(String[] args) // solve a slider puzzle (given below)
}
```

## Solver测试用例


```
public static void main(String[] args) {

// create initial board from file
In in = new In(args[0]);
int n = in.readInt();
int[][] blocks = new int[n][n];
for (int i = 0; i < n; i++)
  for (int j = 0; j < n; j++)
      blocks[i][j] = in.readInt();
Board initial = new Board(blocks);

// solve the puzzle
Solver solver = new Solver(initial);

// print solution to standard output
if (!solver.isSolvable())
  StdOut.println("No solution possible");
else {
  StdOut.println("Minimum number of moves = " + solver.moves());
  for (Board board : solver.solution())
      StdOut.println(board);
}
}
```

##  输出


![8_Puzzle_img6.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/8_Puzzle_img6.jpg)

## 交付和可使用的包

需要提交文件`Board.java`和`Solver.java`，支持官方`args4.jar`，可以使用`java.lang` `java.util`，必须使用args4.jar中的`MinQP`优先级队列。



以上，8Puzzle问题概述完毕。
