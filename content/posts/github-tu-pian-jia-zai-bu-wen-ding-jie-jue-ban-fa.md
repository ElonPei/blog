---
title: GitHub 图片加载不稳定解决办法
date: 2019-04-01
---

打开控制台，发现图片的Get请求部分请求被拒绝。

![GitHub Img](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/oMNqFJ.png)

## 分析原因：

1. 有可能时本地代理拦截了请求

2. 有可能DNS被污染了

## 解决办法：

1. 检查修改本地代理的配置

2. 配置host，如下：

```
199.232.28.133 githubusercontent.com
199.232.28.133 avatars0.githubusercontent.com
199.232.28.133 avatars1.githubusercontent.com
199.232.28.133 avatars2.githubusercontent.com
199.232.28.133 avatars3.githubusercontent.com
199.232.28.133 avatars4.githubusercontent.com
199.232.28.133 avatars5.githubusercontent.com
199.232.28.133 avatars6.githubusercontent.com
199.232.28.133 avatars7.githubusercontent.com
199.232.28.133 avatars8.githubusercontent.com
```
