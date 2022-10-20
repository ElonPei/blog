---
categories:
  - Mac
created_time: September 7, 2022 7:41 PM
date: 2022/01/03
excerpt: " 设置一位数密码"
show_category: Yes
status: 待发布
tags:
  - Mac
updated_time: October 20, 2022 3:18 PM
title: mac os 10.14 取消4位数密码限制
---


1. 打开终端输入

```bash
pwpolicy -clearaccountpolicies
```

1. 然后 `passwd` 可以更改密码