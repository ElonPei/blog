---
title: Git知识总结
date: 2015-03-23
---

## 版本管理


### 版本库


暂存区 >（stage | index）


![git.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/git.jpg)


<!--more-->


### 版本回退

**日志和提交记录查看**


`shell
t reflog
t log
```


**把commit_id所指向的版本之后的提交抛弃**


`shell
t reset --hard commit_id
```


--soft – 缓存区和工作目录都不会被改变

--mixed – 默认选项。缓存区和你指定的提交同步，但工作目录不受影响

--hard – 缓存区和工作目录都同步到你指定的提交


**抛弃commit_id所指向的版本**


`shell
t revert 
```


他会创建一个新的提交，不会影响原有的提交记录。


### 撤销修改

**只在工作区修改**


`shell
t checkout -- {file}
```


**已经add到暂存区了**


`shell
t reset HEAD {file}
```


## 分支

### 创建与合并


**创建分支**


`
t branch test
```


**切换分支**


`
t checkout test
```


> 创建并切换分支

> ```

> git checkout -b test

> ```


**查看当前分支**


`
t branch
```


**合并分支**


`
t merge test
```


表示把test分支合并到当前分支


> 添加 `--no-ff` ，表示禁用`Fast forward`，会产生一条提交记录


**删除分支**


`
t branch -d test
```


### 解决冲突

1. 手动修改冲突文件

2. 再次`add`,`commit`

3. 查看合并情况


`shell
t log --graph
```


### 使用BUG分支


1. 保存工作到一半的现场（储藏功能）`git stash`

2. 切换到需要修改bug的分支

3. 创建bug专用分支，修改bug并提交

4. 合并分支，删除bug分支

5. 列出储藏的工作现场`git stash list`

6. 取出储藏现场`git stash pop`


> `git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除


## 远程相关

**查看远程库信息**


`
t remote -v
```


**推送分支**


`
t push origin master
```


**更新分支**


> clone默认拉取master分支


`
t clone {url}
```


> 拉取远程分支


`
t checkout -b dev origin/dev
```


> 更新


`
t pull
```


## 标签管理


### 打标签(可指定版本号)


`
t tag v1.0 [commit id]
```


### 查看标签


1. 查看列表


`
t tag
```


2. 查看详情


`
t show {标签名}
```


> 可以使用`-s`来私钥签名，也可添加说明。


3. 操作标签


> 删除


`
t tag -d {tag.name}
```


> 推送标签到远程


`
t push origin <tagname>
```


> 全部推送


`
t push origin --tags
```


> 删除远程标签


`
t tag -d <tag_name>
t push origin :refs/tags/<tag_name>
```
