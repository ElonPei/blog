---
title: Git Commit 提交报错
date: 2016-06-01
---

git commit 代码时警告

```
Warning: Your console font probably doesn‘t support Unicode.
If you experience trange characters in the output,
consider switching to a TrueType font such as ucida Console!
```

> 解决方案

这是代码中含有中文导致的，且把代码改为utf-8也是解决不了的，查询了很多资料，最后终于解决啦，现分享给大家： 依次执行以下命令：

```
git config [--global] core.quotepath off
git config [--global] --unset i18n.logoutputencoding
git config [--global] --unset i18n.commitencoding
```
