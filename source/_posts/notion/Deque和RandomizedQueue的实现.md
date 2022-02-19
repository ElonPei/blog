---
categories:
  - 算法与数据结构
created_time: February 19, 2022 9:31 AM
date: 2017/12/11
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Deque和RandomizedQueue的实现
---


本文为《算法4》作者 Robert Sedgewick 和 Kevin Wayne 在 Cursera 上开设的公开课的编程作业，出自WEEK2编程作业，作业详情链接。

[点此查看](http://coursera.cs.princeton.edu/algs4/assignments/queues.html)

## 作业简单描述

具体可去作业详情页查看 1. 实现一个双向队列API如下 2. 实现一个具有随机读取的队列API如下 3. 把标准输入中的数据读入队列，并按要求数量随机输出

## 双向队列实现

### 1. 官方提交测试后遇到的问题

- 迭代器中 `next` 方法和 `remove` 方法对异常的处理。
- Node对象的 `item` 只在声明和构造函数中使用，应当声明为 `final` 类型。
- 在 `removeFirst` 和 `removeLast` 中，需要做必要的非空判断。

### 2. 实现代码

```
public class Deque<Item> implements Iterable<Item> {    private Node<Item> first;    private Node<Item> last;    private int size;    public Deque() {    }    public boolean isEmpty() {        return first == null;    }    public int size() {        return size;    }    public void addFirst(Item item) {        verifyItem(item);        Node<Item> f = first;        first = new Node<>(item, null, f);        if (f == null) {            last = first;        } else {            f.next = first;        }        size++;    }    public void addLast(Item item) {        verifyItem(item);        Node<Item> ol = last;        last = new Node<>(item, ol, null);        if (ol == null) {            first = last;        } else {            ol.prev = last;        }        size++;    }    public Item removeFirst() {        if (isEmpty()) {            throw new NoSuchElementException("the deque is empty");        }        Item item = first.item;        first = first.prev;        if (first != null) {            first.next = null;        } else {            last = null;        }        size--;        return item;    }    public Item removeLast() {        if (isEmpty()) {            throw new NoSuchElementException("the deque is empty");        }        Item item = last.item;        last = last.next;        if (last != null) {            last.prev = null;        } else {            first = null;        }        size--;        return item;    }    @Override    public Iterator<Item> iterator() {        return new DequeIterator<>();    }    private void verifyItem(Item item) {        if (item == null) {            throw new IllegalArgumentException("the item is null");        }    }    private class Node<E> {        private final E item;        private Node<E> next;        private Node<E> prev;        Node(E item, Node<E> next, Node<E> prev) {            this.item = item;            this.next = next;            this.prev = prev;        }    }    private class DequeIterator<E> implements Iterator<E> {        private Node<Item> current = first;        private int i = size;        @Override        public boolean hasNext() {            return i > 0;        }        @SuppressWarnings("unchecked")        @Override        public E next() {            if (!hasNext()) {                throw new NoSuchElementException("the deque no more item to return");            }            Item item = current.item;            current = current.prev;            i--;            return (E) item;        }        @Override        public void remove() {            throw new UnsupportedOperationException("iterator can't remove item");        }    }}
```

## 随机读取队列实现

扩容机制采取增量2倍，减量1/4倍的判断思路。

### 官方提交测试后遇到的问题

因为没有读全原题的本意，在迭代器中使用顺序迭代，在提交测试后报如下错误

```
Test 9: create two nested iterators over the same randomized queue
  * n = 10
    - two inner iterators return the same sequence of items
    - they should return the same set of items but in a
      different order

  * n = 50
    - two inner iterators return the same sequence of items
    - they should return the same set of items but in a
      different order

==> FAILED
```

迭代器数据应当具有随机迭代功能，实现思路在迭代器实现中新建一个数组，用来保存需要迭代数据的数组下标，使用 `StdRandom.shuffle();` 对数组进行shuffle打乱顺序，然后进行迭代操作。

### 实现代码

```
public class RandomizedQueue<Item> implements Iterable<Item> {    private static final int CAPTURE_DOMAIN = 4;    private Object[] data;    private int size;    public RandomizedQueue() {        data = new Object[1];    }    public boolean isEmpty() {        return size == 0;    }    public int size() {        return size;    }    public void enqueue(Item item) {        verifyItem(item);        if (size == data.length) {            resize(data.length * 2);        }        data[size++] = item;    }    @SuppressWarnings("unchecked")    public Item dequeue() {        if (isEmpty()) {            throw new NoSuchElementException("The queue is empty");        }        if (size == data.length / CAPTURE_DOMAIN) {            resize(data.length / 2);        }        int index = StdRandom.uniform(size);        Object item = data[index];        if (index == size - 1) {            data[index] = null;        } else {            data[index] = data[size - 1];            data[size - 1] = null;        }        size--;        return (Item) item;    }    private void resize(int capacity) {        Object[] copy = new Object[capacity];        for (int i = 0; i < size; i++) {            copy[i] = data[i];        }        data = copy;    }    private void verifyItem(Item item) {        if (item == null) {            throw new IllegalArgumentException("The item is null");        }    }    @SuppressWarnings("unchecked")    public Item sample() {        if (isEmpty()) {            throw new NoSuchElementException("The queue is empty");        }        int index = StdRandom.uniform(size);        return (Item) data[index];    }    @Override    public Iterator<Item> iterator() {        return new RandomizedQueueIterator<>();    }    private class RandomizedQueueIterator<E> implements Iterator<E> {        private int pointer;        private int[] shuffleIndex = new int[size];        public RandomizedQueueIterator() {            pointer = 0;            for (int i = 0; i < size; i++) {                shuffleIndex[i] = i;            }            StdRandom.shuffle(shuffleIndex);        }        @Override        public boolean hasNext() {            return pointer < size;        }        @SuppressWarnings("unchecked")        @Override        public E next() {            if (!hasNext()) {                throw new NoSuchElementException("The queue is empty");            }            return (E) data[shuffleIndex[pointer++]];        }        @Override        public void remove() {            throw new UnsupportedOperationException();        }    }    public static void main(String[] args) {        RandomizedQueue<String> queue = new RandomizedQueue<>();        queue.enqueue("a");        queue.enqueue("b");        queue.enqueue("c");        queue.enqueue("d");        StdOut.println(queue.dequeue());        StdOut.println(queue.dequeue());        for (String s : queue) {            StdOut.print(s + " ");        }        StdOut.println();        for (int i = 0; i < 3; i++) {            StdOut.print(queue.sample() + " ");        }    }}
```

## 标准输入流打印实现

```
public class Permutation {    public static void main(String[] args) {        RandomizedQueue<String> queue = new RandomizedQueue<>();        while (!StdIn.isEmpty()) {            queue.enqueue(StdIn.readString());        }        if (args.length > 0 && args[0] != null) {            for (int i = 0; i < Integer.parseInt(args[0]); i++) {                StdOut.println(queue.dequeue());            }        }    }}
```

以上，完美通关！转载请注明出处！