---
title: 单例模式
date: 2022-01-26
tags:
  - 设计模式-创建型
---

# 意图


确保一个类只生成一个对象，并提供该实例的全局访问点。


## 类图


![%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F%2022f2f7f7b4f142b7bfdd25b5484cbffa/Diagram.drawio.svg](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/单例模式.drawio.svg)


## 实现


饿汉式，线程安全，坏处是不节约资源。


```java
private static Singleton uniqueInstance = new Singleton();
```


懒汉式，线程安全的写法，好处是节约资源，坏处是在多线程的环境下，线程会阻塞时间过长，有性能问题，不建议试用该方法。


```java
public static synchronized Singleton getUniqueInstance() {
    if (uniqueInstance == null) {
        uniqueInstance = new Singleton();
    }
    return uniqueInstance;
}
```


双重校验锁，是线程安全的写法，这种写法高并发也没有问题，也是懒加载的。


```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```


volatile 关键字可以防止指令重排序。

`getUniqueInstance()` 中两个if判断必要的，第一个if避免了实例化之后的加锁操作，第二个if避免了多个线程同时实例化的现象。


静态内部类实现，懒加载，JVM能够保证实例只被实例化一次。


```java
public class Singleton {

    private Singleton() {
    }

    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getUniqueInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```


## 使用场景


1. Windows的Task Manager（任务管理器）就是很典型的单例模式.

2. 网站的计数器，一般也是采用单例模式实现，否则难以同步。

3. 应用程序的日志应用，一般都何用单例模式实现，这一般是由于共享的日志文件一直处于打开状态，因为只能有一个实例去操作，否则内容不好追加。

4. Web应用的配置对象的读取，一般也应用单例模式，这个是由于配置文件是共享的资源。

5. 数据库连接池的设计一般也是采用单例模式，因为数据库连接是一种数据库资源。数据库软件系统中使用数据库连接池，主要是节省打开或者关闭数据库连接所引起的效率损耗，这种效率上的损耗还是非常昂贵的，因为何用单例模式来维护，就可以大大降低这种损耗。

6. 多线程的线程池的设计一般也是采用单例模式，这是由于线程池要方便对池中的线程进行控制。

7. 操作系统的文件系统，也是大的单例模式实现的具体例子，一个操作系统只能有一个文件系统。


## 现实中使用的场景


[java.lang.System#getSecurityManager()](http://docs.oracle.com/javase/8/docs/api/java/lang/System.html#getSecurityManager--)

[唯一ID工具-IdUtil](https://www.bookstack.cn/read/hutool/bfd2d43bcada297e.md)
