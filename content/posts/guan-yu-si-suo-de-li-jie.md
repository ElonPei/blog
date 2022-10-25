---
title: 关于死锁的理解
date: 2018-03-01
tags:
  - Java
---

## 基本概念

> 死锁是两个或更多线程阻塞着等待其它处于死锁状态的线程所持有的锁。死锁通常发生在多个线程同时但以不同的顺序请求同一组锁的时候。

```java
/**
 * 包名: com.elong.concurrency.ifeve
 * 创建人 : Elong
 * 时间: 16/9/20 上午11:49
 * 描述 : 死锁的演示
 */
public class TreeNode {
    TreeNode parent   = null;
    List children = new ArrayList();
    public synchronized void addChild(TreeNode child){
        System.out.println(Thread.currentThread().getName() + ": addChild()");
        if(!this.children.contains(child)) {
            this.children.add(child);
            child.setParentOnly(this);
        }
    }
    public synchronized void addChildOnly(TreeNode child){
        System.out.println(Thread.currentThread().getName() + ": addChildOnly()");
        if(!this.children.contains(child)){
            this.children.add(child);
        }
    }
    public synchronized void setParent(TreeNode parent){
        System.out.println(Thread.currentThread().getName() + ": setParent()");
        this.parent = parent;
        parent.addChildOnly(this);
    }
    public synchronized void setParentOnly(TreeNode parent){
        System.out.println(Thread.currentThread().getName() + ": setParentOnly()");
        this.parent = parent;
    }

    public static void main(String[] args) {
        final TreeNode parent = new TreeNode();
        final TreeNode child = new TreeNode();
        Thread t1 = new Thread(){
            @Override
            public void run() {
                parent.addChild(child);
            }
        };
        Thread t2 = new Thread(){
            @Override
            public void run() {
                child.setParent(parent);
            }
        };
        t1.start();
        t2.start();
    }
}
```

执行main方法，线程 `t1` 和线程 `t2` 同时启动，线程 `t1` 调用同步方法 addChild(),取得 parent 对象的锁，并且在 addChild() 方法中尝试获取 child 对象的锁，同时线程 `t2` 调用 setParent() 方法，取得 child 对象的锁，并且在 setParent() 方法中尝试获取 parent 对象的锁，在某种情况下，由于 `t2` 拿着 child 对象的所，所以 `t1` 一直处于阻塞状态，而 `t1` 拿着 parent 对象的锁，`t2` 也会处于阻塞状态，就会导致死锁。

## 如何避免死锁

1. 加锁顺序

2. 加锁时限（自定义锁）

3. 死锁检测

4. 设置锁的优先级

## 参考

http://ifeve.com/deadlock/
