---
title: MacOS 取消4位数密码限制
date: 2022-01-12
tags:
  - Mac
---

> 10.14

1. 打开终端输入

```bash
pwpolicy -clearaccountpolicies
```

2. 然后 `passwd` 可以更改密码