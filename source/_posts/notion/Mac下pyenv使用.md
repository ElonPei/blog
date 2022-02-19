---
categories:
  - Mac
created_time: February 19, 2022 9:30 AM
date: 2018/03/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Mac下pyenv使用
---


### pyenv相关

- `commands` List all available pyenv commands
- `local` Set or show the local application-specific Python version
- `global` Set or show the global Python version
- `shell` Set or show the shell-specific Python version
- `install` Install a Python version using python-build
- `uninstall` Uninstall a specific Python version
- `rehash` Rehash pyenv shims (run this after installing executables)
- `version` Show the current Python version and its origin
- `versions` List all Python versions available to pyenv
- `which` Display the full path to an executable
- `whence` List all Python versions that contain the given executable

### 常用命令

1. 查看可安装版本、安装、更新数据库

```
pyenv install --list
pyenv install #version
pyenv rehash
```

1. 查看已安装的版本

```
pyenv versions
```

1. 设置环境

```
pyenv local #version //设置当前目录环境
pyenv global #version //设置全局环境变量
pyenv shell #version //设置当前会话的环境变量
```

1. 卸载、更新

```
pyenv uninstall #verson //卸载
pyenv update        //更新 pyenv 及其插件
```