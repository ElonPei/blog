---
categories:
  - Python
created_time: February 19, 2022 5:52 PM
date: 2020/07/10
excerpt: 由于MWeb不支持阿里云图床，所以自己做了个脚本，本脚本的主要功能时把图片上传到阿里云oss并把图片的markdown语法复制到粘贴板。
show_category: Yes
status: 待发布
tags:
  - Python
  - 效率
updated_time: February 20, 2022 9:34 AM
title: 阿里云OSS文件上传Python脚本
---


# **oss2mk**

由于`MWeb`不支持阿里云图床，本脚本的主要功能时把图片上传到阿里云oss并把图片的markdown语法复制到粘贴板。

## **运行环境**

```
 系统          : macOS 10.13
 Python版本    : 3.6.1
```

## **使用方法**

执行命令

```
  oss_2_mk.py [-absolute_path]
```

例:

```
  执行:
  oss_2_mk.py /Users/elong/Desktop/war/gc_collector.jxpxg
  输出:
  obj_name : gc_collector.jxpxg
  start upload to oss ....
  end upload to oss
  please CMD + v to paste markdown img grammar
```

## 原**代码**

源代码已开源，**[点此查看](https://github.com/erick-pei/oss2mk/blob/master/oss_2_mk.py)**。