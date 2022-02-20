---
categories:
  - Android
created_time: February 19, 2022 9:31 AM
date: 2015/03/16
show_category: No
status: 待发布
updated_time: February 20, 2022 9:38 AM
title: Eclipse运行Android项目时Failed to install SqliteDemo.apk on device '6CY4255XR9!
---


## ### 问题描述

> 今天写了一个关于Sqlite的Demo，运行项目到手机的时候安装时间很长，最后控制台报如下错误，网上查询没有解决了，写下这篇博客做一下记录。
> 

```
Failed to install SqliteDemo.apk on device '6CY4255XR9!
```

## ### 解决办法

> 执行 Project–>Clear ，然后尝试运行项目。重启Eclipse， 尝试运行重启adb 执行： 开始–>运行–>cmd–>adb kill-server –> adb start-server模拟器用户：删除并重启手机用户：重启手机
>