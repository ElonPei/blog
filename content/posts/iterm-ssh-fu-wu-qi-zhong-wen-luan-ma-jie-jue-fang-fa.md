---
title: iTerm SSH 服务器中文乱码解决方法
date: 2021-08-04
tags:
  - tools
  - macOS
---


在macOS下，编辑`.zshrc`配置文件。

```
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
```

加入后退出再次进入，解决。
