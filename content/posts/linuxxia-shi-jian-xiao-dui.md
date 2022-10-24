---
title: Linux下时间校对
date: 2017-11-01
---

## 1. 使用 `yum`安装 `ntp`插件

```
yum install ntp
```

## 2. 执行命令同步时间

```
ntpdate cn.pool.ntp.org
```

`cn.pool.ntp.org`是 `ntp`网络授时组织的中国授时源。
