---
title: GitHub Action 报错解决
date: 2022-04-29
---

## 问题

之前 Action 一直运行的好好的，突然就报错了，我猜测可能是 GitHub 修改了 `workflow_dispatch` 的参数，导致原参数不可用了。

```jsx
The workflow is not valid. .github/workflows/xxx.yml (Line: x, Col: x): Unexpected value 'branches'
```

## 解决方式

删除分支相关的信息，在手动触发的时候因为有分支信息，所以不用担心取不到对应的分支的代码。
