---
title: Java NIO相关知识总结
date: 2018-03-01
tags:
  - Java
---



### 1. 缓冲区Buffer


Buffer是一个对象，它包含一些要写入或者读出的数据。在面向流的IO中，可以将数据直接写入或将数据直接读到`Stream`对象中。缓冲区实质上是一个数组。通常是一个字节数组。同时还维护着读写位置等信息，并提供了对数据的结构化访问能力。


每一种`Buffer`类都是`Buffer`接口的子类型，除了`Boolean`类型，每一种Java的基本类型都有对应的Buffer


ByteBuffer

CharBuffer

DoubleBuffer

FloatBuffer

IntBuffer

LongBuffer

ShortBuffer


其中`ByteBuffer`要特殊一些，它在具有一般缓冲区的操作之外还提供了一些特有的操作，以方便网络读写。


### 2.通道Channel


一般情况下，所有的IO在NIO中都是从一个Channel开始的，Channel是一个通道（有点像流），可以把Channel读到Buffer中，也可以从Buffer写到Channel中。


![http://ifeve.com/wp-content/uploads/2013/06/overview-channels-buffers1.png](http://ifeve.com/wp-content/uploads/2013/06/overview-channels-buffers1.png)


http://ifeve.com/wp-content/uploads/2013/06/overview-channels-buffers1.png


### Chanel的主要实现


FileChannel

DatagramChannel

SocketChannel

ServerSocketChannel


这些实现覆盖了UDP和TCP的网络IO，以及文件的IO。


### 3.选择器Selector


Selector允许单线程处理多个Channel。如果你的应用程序打开了多个链接（通道），但每个链接的流量都很低，使用Selector就很方便。例如在一个聊天的服务器。


这是在一个单线程中使用一个Selector处理3个Channel的图示：


![http://ifeve.com/wp-content/uploads/2013/06/overview-selectors.png](http://ifeve.com/wp-content/uploads/2013/06/overview-selectors.png)


http://ifeve.com/wp-content/uploads/2013/06/overview-selectors.png


要使用Selector，就得向Selector注册Channel，然后调用他的select()方法。这个方法就会一直阻塞到某个注册的通道有事件就绪。一旦这个方法返回，线程就可以处理这些事件，事件的例子有如新的链接进来，数据的接收等。


> 部分转载自并发编程网 – ifeve.com

>
