---
title: 阿里云OSS文件上传Python脚本
date: 2020-07-10
tags:
  - Python
  - 效率
---

## oss2mk

由于`MWeb`不支持阿里云图床，本脚本的主要功能时把图片上传到阿里云oss并把图片的markdown语法复制到粘贴板。

## 运行环境

 ```
  系统          : macOS 10.13
  Python版本    : 3.6.1
```

## 使用方法

执行命令

 ```shell
 oss_2_mk.py [-absolute_path]
```

例

 ```shell
 执行:
 oss_2_mk.py /Users/elong/Desktop/war/gc_collector.jxpxg
 输出:
 obj_name : gc_collector.jxpxg
 start upload to oss ....
 end upload to oss
 please CMD + v to paste markdown img grammar
```

## 原代码

源代码已开源，**[点此查看](https://github.com/erick-pei/oss2mk/blob/master/oss_2_mk.py)**。
