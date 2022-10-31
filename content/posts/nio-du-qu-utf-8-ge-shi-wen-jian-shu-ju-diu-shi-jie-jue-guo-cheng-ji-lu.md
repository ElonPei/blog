---
title: NIO 读取 UTF-8 格式文件数据丢失解决过程记录
date: 2018-08-01
tags:
  - Java
---


## 问题描述

使用NIO读取一个文件时，从`Channel`中读取数据到`ByteBuffer`中，指定`ByteBuffer`的大小为1024，当读取一个大于1024的文件是，至少需要2+n的读取次数。

对于`utf-8`格式的文件，直接读取会产生乱码，为解决乱码，使用`CharsetDecoder`的`decode(ByteBuffer in, CharBuffer out,  boolean endOfInput)`方法进行编码，乱码问题解决。

但是输出内容相对于源文件有数据缺失现象，一步一步跟debug后，发现decode方法返回`MALFORMED[1]`错误代码。

## 问题解决思路

在进行一番Google后，定位到了问题是由于每次编码的第一个字节不能为utf-8格式的。找到每次读取的边界，添加几个字母后数据不丢失了，这种方法显然是行不通的。Google到的内容如下：

来自: http://stackoverflow.com/questions/29559842/what-is-charsetdecoder-decodebytebuffer-charbuffer-endofinput

```
Apparently it does not work correctly if you start decoding in the middle of a character. The decoder expects that the first thing it reads is the start of a valid UTF-8 sequence.

edit - When the decoder reports UNDERFLOW, it expects you to add more data to the input buffer and then try to call decode() again, but you must re-offer it the data from the start of the UTF-8 sequence that you are trying to decode. You can't continue in the middle of an UTF-8 sequence.
```

后来又搜索到另一个答案：

来自：http://stackoverflow.com/questions/14792654/decoding-multibyte-utf8-symbols-with-charset-decoder-in-byte-by-byte-manner

```
If my theory is correct then the answer is that you cannot decode UTF-8 by providing the decoder with byte buffers containing exactly one byte. But you could implement byte-by-byte decoding by starting with a ByteBuffer containing one byte and adding extra bytes until the decoder succeeds in outputing a character. Just make sure that you don't clobber input bytes that haven't been consumed yet.
  Note that decoding like this is not efficient. The API design is optimized for decoding a large number of bytes in one go.
```

## 思考

第二段答案中最后一句话：最佳化API设计是每一次编码使用一个较大的bytes接收。

修改ByteBuffer的初始化大小后问题解决。怪怪得(￣∇￣)
