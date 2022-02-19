---
categories:
  - 操作系统
created_time: February 19, 2022 9:30 AM
date: 2017/11/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Linux下时间校对
---


# 1. 使用 `yum`安装 `ntp`插件：

```
yum install ntp
```

# 2. 执行命令同步时间：

```
ntpdate cn.pool.ntp.org
```

`cn.pool.ntp.org`是 `ntp`网络授时组织的中国授时源。