---
title: macOS 编译 Nginx 1.12.1 报错解决
date: 2018-01-01
tags:
  - macOS
  - Nginx
---


## 编译报错日志

```
dyld: Library not loaded: /usr/local/lib/libluajit-5.1.2.dylib
```

## 解决办法

在Nginx源代码目录中修改以下文件

```
vim objs/Makefile
```

找到如下内容

```
&& ./config --prefix=/Users/elong/Applications/build_openresty/nginx-1.12.1/../openssl-1.0.1p/.ope     nssl no-shared
```

将其中 `./config` 修改为 `./Configure darwin64-x86_64-cc`，注意修改后不要再次执行 `configure` 命令。

## 原因

nginx 在调用 openssl 的源码编译时, 调错了 configure 文件, 最终没能正确编译出需要的 openssl x86_64 库文件。

## 参考

参考书籍：《Mastering NGINX 2nd Edition》

参考文章：https://www.widlabs.com/article/mac-os-x-nginx-compile-symbol-not-found-for-architecture-x86_64
