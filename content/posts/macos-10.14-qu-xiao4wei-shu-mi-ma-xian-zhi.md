---
title: MacOS 10.14 取消4位数密码限制
date: 2022-01-03
tags:
  - macOS
---


1. 打开终端输入

```bash
pwpolicy -clearaccountpolicies
```

2. 然后 `passwd` 可以更改密码
