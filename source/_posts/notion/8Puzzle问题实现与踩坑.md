---
categories:
  - 算法与数据结构
created_time: February 19, 2022 9:31 AM
date: 2018/01/23
excerpt: 8Puzzle问题算法使用A*算法，数据结构主要是优先队列。
show_category: Yes
status: 待发布
tags:
  - Algorithm
updated_time: February 19, 2022 11:12 AM
title: 8Puzzle问题实现与踩坑
---


> 8Puzzle问题算法使用A*算法，数据结构主要是优先队列。
> 

## 评分系统错误修复记录

### Board类toString实现丢失部分打印

解决方案，添加即可：

```java
public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append(blocks.length).append("\n");
    for (int[] block : blocks) {
        for (int j = 0; j < blocks.length; j++) {
            sb.append(" ").append(block[j]);
        }
        sb.append("\n");
    }
    return sb.toString();
}
```

添加对对象的类的判断，添加后即可：

```java
public boolean equals(Object other) {
    if (other == null) {
        return false;
    }
    if (other == this) {
        return true;
    }
    if (other.getClass() != this.getClass()) {
        return false;
    }
    if (other instanceof Board) {
        Board that = (Board) other;
        if (that.blocks.length != this.blocks.length) {
            return false;
        }
        for (int i = 0; i < this.blocks.length; i++) {
            for (int j = 0; j < this.blocks.length; j++) {
                if (this.blocks[i][j] != that.blocks[i][j]) {
                    return false;
                }
            }
        }
    }
    return true;
}
```

解决方案，swap 调用方错误，修改为 board 引用即可。

```java
public Board twin() {
    Board board = new Board(blocks);
    for (int i = 0; i < blocks.length; i++) {
        for (int j = 0; j < blocks.length - 1; j++) {
            if (board.blocks[i][j] != 0 && board.blocks[i][j + 1] != 0) {
                board.swap(i, j, i, j + 1);
                return board;
            }
        }
    }
    return board;
}
```

最终，终于评分满分。

## 完整实现代码

```java
import edu.princeton.cs.algs4.Stack;
import edu.princeton.cs.algs4.StdOut;

public class Board {
    private final int[][] blocks;

    // construct a board from an n-by-n array of blocks
    // (where blocks[i][j] = block in row i, column j)
    public Board(int[][] blocks) {
        this.blocks = new int[blocks.length][blocks.length];
        for (int i = 0; i < blocks.length; i++) {
            for (int j = 0; j < blocks.length; j++) {
                this.blocks[i][j] = blocks[i][j];
            }
        }
    }

    // board dimension n
    public int dimension() {
        return blocks.length;
    }

    // number of blocks out of place
    public int hamming() {
        int hammingNumber = 0;
        for (int i = 0; i < blocks.length; i++) {
            for (int j = 0; j < blocks.length; j++) {
                if (blocks[i][j] != getGoalVal(i, j) && !isEnd(i, j)) {
                    hammingNumber++;
                }
            }
        }
        return hammingNumber;
    }

    // sum of Manhattan distances between blocks and goal
    public int manhattan() {
        int manhattanNumber = 0;
        for (int i = 0; i < blocks.length; i++) {
            for (int j = 0; j < blocks.length; j++) {
                int value = blocks[i][j];
                if (value != 0 && value != getGoalVal(i, j)) {
                    int ii = (value - 1) / blocks.length;
                    int jj = value - ii * blocks.length - 1;
                    int distance = Math.abs(i - ii) + Math.abs(j - jj);
                    manhattanNumber += distance;
                }
            }
        }
        return manhattanNumber;
    }

    // is this board the goal board?
    public boolean isGoal() {
        if (blocks[blocks.length - 1][blocks.length - 1] != 0) {
            return false;
        }
        for (int i = 0; i < blocks.length; i++) {
            for (int j = 0; j < blocks.length; j++) {
                if (blocks[i][j] != getGoalVal(i, j)) {
                    return false;
                }
            }
        }
        return true;
    }

    // a board that is obtained by exchanging any pair of blocks
    public Board twin() {
        Board board = new Board(blocks);
        for (int i = 0; i < blocks.length; i++) {
            for (int j = 0; j < blocks.length - 1; j++) {
                if (board.blocks[i][j] != 0 && board.blocks[i][j + 1] != 0) {
                    board.swap(i, j, i, j + 1);
                    return board;
                }
            }
        }
        return board;
    }

    // does this board equal y?
    public boolean equals(Object other) {
        if (other == null) {
            return false;
        }
        if (other == this) {
            return true;
        }
        if (other.getClass() != this.getClass()) {
            return false;
        }
        if (other instanceof Board) {
            Board that = (Board) other;
            if (that.blocks.length != this.blocks.length) {
                return false;
            }
            for (int i = 0; i < this.blocks.length; i++) {
                for (int j = 0; j < this.blocks.length; j++) {
                    if (this.blocks[i][j] != that.blocks[i][j]) {
                        return false;
                    }
                }
            }
        }
        return true;
    }

    private int getGoalVal(int i, int j) {
        if (isEnd(i, j)) {
            return 0;
        }
        return i * blocks.length + j + 1;
    }

    private boolean isEnd(int i, int j) {
        return i == blocks.length - 1 && j == blocks.length - 1;
    }

    private boolean swap(int i, int j, int si, int sj) {
        if (si < 0 || si >= blocks.length || sj < 0 || sj >= blocks.length) {
            return false;
        }
        int swap = blocks[i][j];
        blocks[i][j] = blocks[si][sj];
        blocks[si][sj] = swap;
        return true;
    }

    // all neighboring boards
    public Iterable<Board> neighbors() {
        int i0 = 0, j0 = 0;
        boolean isFind = false;
        for (int i = 0; i < blocks.length; i++) {
            for (int j = 0; j < blocks.length; j++) {
                if (blocks[i][j] == 0) {
                    i0 = i;
                    j0 = j;
                    isFind = true;
                    break;
                }
            }
            if (isFind) {
                break;
            }
        }
        Stack<Board> stack = new Stack<>();
        Board board = new Board(blocks);
        boolean isSwap = board.swap(i0, j0, i0 - 1, j0);
        if (isSwap) {
            stack.push(board);
        }
        board = new Board(blocks);
        isSwap = board.swap(i0, j0, i0 + 1, j0);
        if (isSwap) {
            stack.push(board);
        }
        board = new Board(blocks);
        isSwap = board.swap(i0, j0, i0, j0 - 1);
        if (isSwap) {
            stack.push(board);
        }
        board = new Board(blocks);
        isSwap = board.swap(i0, j0, i0, j0 + 1);
        if (isSwap) {
            stack.push(board);
        }
        return stack;
    }

    // string representation of this board (in the output format specified below)
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append(blocks.length).append("\n");
        for (int[] block : blocks) {
            for (int j = 0; j < blocks.length; j++) {
                sb.append(" ").append(block[j]);
            }
            sb.append("\n");
        }
        return sb.toString();
    }

    // unit tests (not graded)
    public static void main(String[] args) {
        int[][] is = new int[3][3];
        int start = 0;
        for (int i = 0; i < is.length; i++) {
            for (int j = 0; j < is.length; j++) {
                is[i][j] = start++;
            }
        }
        Board board = new Board(is);
        StdOut.println(board);
    }

}

```

```java
import edu.princeton.cs.algs4.In;
import edu.princeton.cs.algs4.MinPQ;
import edu.princeton.cs.algs4.Stack;
import edu.princeton.cs.algs4.StdOut;

public class Solver {

    private final Stack<Board> boards;
    private boolean isSolvable;

    // find a solution to the initial board (using the A* algorithm)
    public Solver(Board initial) {
        if (initial == null) {
            throw new IllegalArgumentException("args board is null");
        }
        boards = new Stack<>();
        if (initial.isGoal()) {
            isSolvable = true;
            boards.push(initial);
            return;
        }
        if (initial.twin().isGoal()) {
            isSolvable = false;
            return;
        }

        MinPQ<SearchNode> minPQ = new MinPQ<>();
        MinPQ<SearchNode> minPQTwin = new MinPQ<>();
        Board board = initial;
        Board boardTwin = initial.twin();
        SearchNode node = new SearchNode(board, 0, null);
        SearchNode nodeTwin = new SearchNode(boardTwin, 0, null);
        minPQ.insert(node);
        minPQTwin.insert(nodeTwin);

        while (true) {
            node = minPQ.delMin();
            nodeTwin = minPQTwin.delMin();
            board = node.board;
            boardTwin = nodeTwin.board;
            if (board.isGoal()) {
                isSolvable = true;
                boards.push(board);
                while (node.previous != null) {
                    node = node.previous;
                    boards.push(node.board);
                }
                return;
            }
            if (boardTwin.isGoal()) {
                isSolvable = false;
                return;
            }
            node.moves++;
            nodeTwin.moves++;

            Iterable<Board> neighbors = board.neighbors();
            for (Board b : neighbors) {
                if (node.previous != null && node.previous.board.equals(b)) {
                    continue;
                }
                minPQ.insert(new SearchNode(b, node.moves, node));
            }

            Iterable<Board> neighborsTwin = boardTwin.neighbors();
            for (Board b : neighborsTwin) {
                if (nodeTwin.previous != null && nodeTwin.previous.board.equals(b)) {
                    continue;
                }
                minPQTwin.insert(new SearchNode(b, nodeTwin.moves, nodeTwin));
            }

        }

    }

    // is the initial board solvable?
    public boolean isSolvable() {
        return isSolvable;
    }

    // min number of moves to solve initial board; -1 if unsolvable
    public int moves() {
        if (isSolvable) {
            return boards.size() - 1;
        }
        return -1;
    }

    // sequence of boards in a shortest solution; null if unsolvable
    public Iterable<Board> solution() {
        if (isSolvable) {
            return boards;
        }
        return null;
    }

    private class SearchNode implements Comparable<SearchNode> {
        private Board board;
        private int moves;
        private SearchNode previous;
        private int cachedPriority = -1;

        public SearchNode(Board board, int moves, SearchNode previous) {
            this.board = board;
            this.moves = moves;
            this.previous = previous;
        }

        private int priority() {
            if (cachedPriority == -1) {
                cachedPriority = board.manhattan() + moves;
            }
            return cachedPriority;
        }

        @Override
        public int compareTo(SearchNode that) {
            return Integer.compare(this.priority(), that.priority());
        }

    }

    // solve a slider puzzle (given below)
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
}

```

以上，8Puzzle实现完成。