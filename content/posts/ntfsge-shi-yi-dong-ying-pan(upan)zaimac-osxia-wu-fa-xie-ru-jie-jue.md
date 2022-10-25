---
title: NTFS格式移动硬盘(U盘)在Mac OS下无法写入解决
date: 2017-12-01
tags:
  - Mac
---

> 由于OS X不支持NTFS格式硬盘的写入，最方便的方法就是把系统的mount_ntfs默认加载方式由只读改为读写

## 如何查看移动硬盘的格式

在终端执行查看磁盘命令：

```
diskutil list
```

显示硬盘的格式是NTFS。

## 操作流程

在终端执行进入root权限shell环境中（root权限请谨慎操作）

```
sudo -s
```

第一步：进入sbin目录

```
cd /sbin
```

第二步：重命名系统自带挂载程序

```
mv mount_ntfs mount_ntfs_orig
```

第三步：新建我们我挂载的脚本

```
vim mount_ntfs
```


#!/bin/sh /sbin/mount_ntfs_orig -o rw,nobrowse “$@”

第四步：保存退出，添加可执行权限

```
  chmod 755 mount_ntfs
```

退出root身份

```
  exit
```

## 注意事项

在`mount_ntfs`文件中，`nobrowse`字段会使硬盘在Finder导航中不可见，硬盘在Mac中默认挂载在`/Volumes`隐藏目录下， 可以使用快捷键`CMD+Shift+G`打开跳转，输入`/Volumes`回车，可看到挂载的硬盘。可以使用以下命令解决：

取消隐藏命令

```
  sudo chflags nohidden /Volumes
```

创建快捷方式命令

```
  sudo ln -s [源目录] [快捷方式目录]
```

## 参考相关文章

http://my.oschina.net/iepac/blog/656264

https://www.zhihu.com/question/19571334
