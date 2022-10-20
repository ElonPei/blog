---
title: MacOS 取消4位数密码限制
date: 2022-01-12
tags:
  - Mac
---

> 使用的 MacOS 版本 10.14

```sh
# 1. 打开终端输入
pwpolicy -clearaccountpolicies
# 2. 然后 `passwd` 可以更改密码
passwd
```
