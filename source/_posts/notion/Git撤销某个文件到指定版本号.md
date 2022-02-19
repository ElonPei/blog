---
categories:
  - 开发工具
created_time: February 19, 2022 9:31 AM
date: 2015/04/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Git撤销某个文件到指定版本号
---


1. 撤销文件到更新到index中

```
git reset {version_id} {path}
```

1. 提交

```
git commit
```

1. 检出到工作区

```
git checkout {file}
```

1. 提交到远程库

```
git push origin XXX
```