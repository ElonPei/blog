---
categories:
  - Java
created_time: February 19, 2022 9:30 AM
date: 2018/03/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Java NIO 之 FileChannel 的使用
---


## 通过文件获取 FileChannel 实例

### 1. 从 FileInputstream 中获取

```
FileInputStream fis = new FileInputStream("/Users/peiel/a.txt");FileChannel fileChannel = fis.getChannel();
```

### 2. 使用 RandomAccessFile 获取

```
RandomAccessFile fis = new RandomAccessFile("/Users/peiel/a.txt", "rw");FileChannel fileChannel = fis.getChannel();
```

## 打印读取 FileChannel 中的内容

```
ByteBuffer buf = ByteBuffer.allocate(4);int len;while ((len = fileChannel.read(buf)) != -1) {    // flip buf (limit = position; position = 0;)    buf.flip();    // 方式一：转换成数组读取的方式    System.out.print(new String(buf.array(), 0, len));    // 方式二：直接读取的方式    while (buf.hasRemaining()) {        System.out.print((char) buf.get());    }    // clear    buf.clear();}
```

## 写入数据到文件中

```
String newData = "This is a boy!";ByteBuffer byteBuffer = ByteBuffer.allocate(16);byteBuffer.clear();byteBuffer.put(newData.getBytes());byteBuffer.flip();while (byteBuffer.hasRemaining()) {    fileChannel.write(byteBuffer);}
```