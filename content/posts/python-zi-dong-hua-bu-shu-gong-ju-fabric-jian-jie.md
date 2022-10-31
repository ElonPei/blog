---
title: Python 自动化部署工具 Fabric 简介
date: 2017-01-04
tags:
  - Python
---


## Install

### 准备

python 3.5

pip 3.5

### 安装

```shell
sudo pip3.5 install fabric
```

## 常用命令

```shell
fab ...     # 执行
fab --help
fab -l      # 显示可用的task（命令）
fab -H      # 指定host，支持多个host，以逗号分开
fab -R      # 指定role，支持多个role
fab -P      # 并发数，默认串行
fab -w      # warn_only，默认遇到异常直接abort退出
fab -f      # 指定入口文件，默认fabfile.py
```

## 常用函数

```shell
lcd('/tmp')         # 切换本地目录
cd('/tmp')          # 切换远程目录

local('pwd')        # 执行本地命令
run('uname -a')     # 执行远程命令

sudo('/etc/init.d/nginx start')     # 执行远程sudo
```

## 远程服务器定义

```shell
host1 = 'user@192.168.1.1:22'
host2 = 'user@192.168.1.2:22'
host3 = 'user@192.168.1.3:22'

env.hosts = [ host1, host2, host3 ]

env.passwords = {
    host1: "pwd_of_host1",
    host2: "pwd_of_host2",
    host3: "pwd_of_host3",
}
```

或者

```shell
env.roledefs = {
    'nginx_user': [host1, host2],
    'mysql_user': [host3]
}

env.passwords = {
    host1: "pwd_of_host1",
    host2: "pwd_of_host2",
    host3: "pwd_of_host3",
}
```

ssh keyfile

```shell
env.key_filename = ['/opt/fab/server_key']
env.user = 'tomcat'
env.password = '111111'
env.port = '2862'
```

## 目录的切换

```shell
with cd('/opt/tomcat')	# 远程
    run('set -m ;  ./bin/startup.sh')
with lcd('/opt/tomcat') # 本地
    run('pwd')
```

## 上传下载文件

```python
get('/remote/path/','/local/path/')
put('/local/path/','/remote/path')
```

使用的是`sftp协议`

## 判断文件或目录是否存在

```python
from fabric.contrib.files import exists
    
if exists('/opt/tomcat/logs/catalina.out'):
    print 'catalina.out exist'    
else:
    print 'catalina.out not exist'
```

## DEMO

在本地打包maven项目，并上传到服务器部署。

```python
from fabric.api import *
import time

env.hosts = ['root@192.168.0.1:22']  # ip 端口
env.password = 'password'  # 密码


# local() 执行本地命令
def mvn_pack():
    print("------------------------------------ 开始打包项目 ------------------------------------")
    with lcd("/Users/elong/IdeaProjects/****/"):  # 进入本地目录
        local("mvn clean")
        local("mvn package")


# run() 执行远程服务器的命令
def put_pack():
    print("------------------------------------ 备份服务器原项目 ------------------------------------")
    run("mv ... ..." + time.strftime("%m%d", time.localtime()))
    print("------------------------------------ 开始上传项目包 ------------------------------------")
    put("本地路径", "远程路径")
    print("------------------------------------ 重启Tomcat并打印日志 ------------------------------------")
    run("set -m ; /etc/init.d/tomcat restart")
    time.sleep(2)
    run("tail -f /home/tomcat/logs/catalina.out")


def start():
    mvn_pack()
    put_pack()
```

使用自动部署，可以省去每天重复的事情，只需要看下日志是否启动成功即可，提高了生产效率。也可配合`Jenkins`实现可视化的自动部署、持续集成等工作。

## 参考文章

http://blog.csdn.net/sijiazhaiyuan/article/details/23884873
