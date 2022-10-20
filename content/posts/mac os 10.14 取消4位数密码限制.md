---
title: mac os 10.14 取消4位数密码限制
date: 2022-01-12
tags:
  - Mac
---

1. 打开终端输入

```bash
pwpolicy -clearaccountpolicies
```

1. 然后 `passwd` 可以更改密码