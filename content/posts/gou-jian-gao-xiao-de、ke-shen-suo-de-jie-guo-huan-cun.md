---
title: 构建高效的、可伸缩的结果缓存
date: 2020-07-10
---

⚡️几乎所有的服务器应用程序都需要缓冲，来提高程序的吞吐量和性能，但却需要消耗更多的内存。本文尝试自定义一个高效的、可伸缩的结果缓存。并记录下构建和学习的过程。

## 方案一：使用HashMap来构建结果缓存

使用HashMap来存储数据，由于HashMap不是线程安全的，需要在compute方法上加锁，来保证每次只有一个线程访问缓存。

```java
 public interface Computable<A, V> {
   V compute(A arg) throws InterruptedException;
 }
```

```java
 public class Cacher1<A, V> implements Computable<A, V>{
   private final Map<A, V> cache = new HashMap<>();
   private final Computable<A, V> computable;

   public Cacher1(Computable<A, V> computable) {
       this.computable = computable;
   }

   @Override
   public synchronized V compute(A arg) throws InterruptedException {
       V value = cache.get(arg);
       if (value == null) {
           value = computable.compute(arg);
           cache.put(arg, value);
       }
       return value;
   }
 }
```

本方案暴露出来的问题是性能低，容易阻塞大量线程。

## 方案二：使用ConcurrentHashMap来构建结果缓存

使用JDK提供的`HashMap`的同步实现类`ConcurrentHashMap`来提升缓存的并发性。

```java
 public class Cacher2<A, V> implements Computable<A, V>{
   private final Map<A, V> cache = new ConcurrentHashMap<>();
   private final Computable<A, V> computable;

   public Cacher2(Computable<A, V> computable) {
       this.computable = computable;
   }

   @Override
   public V compute(A arg) throws InterruptedException {
       V value = cache.get(arg);
       if (value == null) {
           value = computable.compute(arg);
           cache.put(arg, value);
       }
       return value;
   }
 }
```

相对于第一种方案，并发性能能得到明显的提升，但是会造成重复计算相同的结果，两个线程同时获取同一个结果的时候，都进入耗时计算过程，计算出相同的结果，增加了没必要的计算负担。

## 方案三：使用FutureTask异步进行耗时运算

`FutureTask`实现了`Future`接口，表示一种抽象的，可生成结果的计算。`FutureTask`表示的计算是通过Callable来实现的，相当于一种可生成结果的Runnable。使用`FutureTask`异步进行耗时运算， 缓存采用先缓存再计算的方式。

```java
 public class Cacher3<A, V> implements Computable<A, V>{
   private final Map<A, Future<V>> cache = new ConcurrentHashMap<>();
   private final Computable<A, V> computable;

   public Cacher3(Computable<A, V> computable) {
       this.computable = computable;
   }

   @Override
   public V compute(final A arg) throws InterruptedException {
       Future<V> future = cache.get(arg);
       if (future == null) {
           Callable<V> callable = new Callable<V>() {
               @Override
               public V call() throws InterruptedException {
                   return computable.compute(arg);
               }
           };
           FutureTask<V> ft = new FutureTask<>(callable);
           future = ft;
           cache.put(arg, future);
           ft.run();
       }
       try {
           return future.get();
       } catch (ExecutionException e) {
           throw new InterruptedException(e.getMessage());
       }
   }

 }
```

还是会造成重复计算相同的结果，两个线程同时获取同一个结果的时候，都进入耗时计算过程，计算出相同的结果，增加了没必要的计算负担。

## 方案四：Cacher的最终实现

使用`ConcurrentHashMap`的`putIfAbsent`原子的进行缓存，来解决重复计算的问题。

```java
 public class Cacher<A, V> implements Computable<A, V>{
   private final ConcurrentMap<A, Future<V>> cache = new ConcurrentHashMap<>();
   private final Computable<A, V> computable;

   public Cacher(Computable<A, V> computable) {
       this.computable = computable;
   }

   @Override
   public V compute(final A arg) throws InterruptedException {
       Future<V> future = cache.get(arg);
       if (future == null) {
           Callable<V> callable = new Callable<V>() {
               @Override
               public V call() throws InterruptedException {
                   return computable.compute(arg);
               }
           };
           FutureTask<V> ft = new FutureTask<>(callable);
           future = ft;
           cache.putIfAbsent(arg, future);
           ft.run();
       }
       try {
           return future.get();
       } catch (ExecutionException e) {
           throw new InterruptedException(e.getMessage());
       }
   }

 }
```

此缓存没有对缓存的过期做处理。

## 关于并发编程的总结

- 可变状态是指环重要的。

- 尽量将域声明为final类型，除非需要它们是可变的。

- 不可变对象一定是线程安全的。

- 封装有助于管理复杂性。

- 用锁来保护每个可变变量。

- 当保护同一个不变性条件中的所有变量时，要使用同一个锁。

- 在执行复合操作期间，要持有锁。

- 如果从多个线程中访问同一个可变变量时没有同步机制，那么程序会出现问题。

- 不要故作聪明的推断出不需要使用同步。

- 在设计过程中考虑线程安全，或者在文档中明确的指出它不是线程安全的。

- 将同步策略文档化。
