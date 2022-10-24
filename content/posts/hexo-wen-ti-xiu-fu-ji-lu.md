---
title: Hexo 问题修复记录
date: 2016-06-01
---

## 安装 nvm


1. `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash`

2. 配置环境变量

## 使用 nvm 快捷安装 node


`
m install 6.17.0
```

## 报错解决

### 错误1


`
ROR Plugin load failed: hexo-renderer-sass
```

### 解决1


使用brew安装系统 `libsass` 依赖


`
ew reinstall libsass
```

### 错误2


node版本与编译版本不一致导致失败


`
ror: The module '/usr/local/lib/node_modules/hexo/node_modules/dtrace-provider/build/Release/DTraceProviderBindings.node'
s compiled against a different Node.js version using
DE_MODULE_VERSION 48. This version of Node.js requires
DE_MODULE_VERSION 67. Please try re-compiling or re-installing
```

### 解决2


根据错误日志，查看版本号48的node版本，[点击查看链接](https://nodejs.org/zh-cn/download/releases/)


![http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/node_verison.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/node_verison.jpg)


node_verison.jpg


使用nvm安装对应版本：


`
m install 6.17.0
```

## 总结


hexo 不能使用的主要原因还是设备更换的原因，更换设备注意同步node版本，环境依赖，就不会出问题。
