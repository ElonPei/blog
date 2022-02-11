---
categories:
  - Mac
date: 2022/01/16
excerpt: 作为Java程序员，在M1芯片的Mac平台工作已经接近两年，近期购入M1 Pro的MacBook
  Pro后，简单折腾和优化了下开发环境，并记录本文章，供后续查看。
show_category: Yes
status: 待发布
tags:
  - Mac
title: 基于M1芯片的Mac环境搭建记录
---


<aside>

<img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 作为Java程序员，在M1芯片的Mac平台工作已经接近两年，近期购入M1 Pro的MacBook Pro后，简单折腾和优化了下开发环境，并记录本文章，供后续查看。
</aside>

## 软件

### iTem2

> Mac上比较好用的终端工具
> 
- 支持m1

### Dash

> Mac上比较好用的文档管理工具
> 
- 支持m1

## 开发环境

### Homebrew

- 支持m1

```bash
/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"
```

### brew安装MySQL 环境

- 支持m1

```bash
brew install mysql
```

### brew安装conda环境安装

> anaconda 官方暂时不支持 m1，只能通过转译来运行，开源的 miniforge 支持 arm 架构，我们使用 brew 来安装这个版本。
> 

 

```bash
brew install miniforge
# 如果安装zsh环境，需要执行如下初始化
conda init zsh
# 如果中断是shell环境，执行如下命令
conda init
```

### Pytnon环境及常用依赖安装

> 安装 conda 环境后，我们就可以管理我们的 python 环境了。
> 

```bash
# 安装pandas
conda install pandas
# 安装numpy
conda install numpy
# 安装rich
conda install rich
# 安装mysql支持
conda install PyMySQL
```

### Docker启动Nacos环境

```bash
# 1. 拉取镜像
docker pull zhusaidong/nacos-server-m1:2.0.3
# 2. 启动服务
docker run --name nacos-standalone -e MODE=standalone -e JVM_XMS=512m -e JVM_XMX=512m -e JVM_XMN=256m -p 8848:8848 -d zhusaidong/nacos-server-m1:2.0.3
```

### Docker启动Nginx环境

```bash
docker run --name my-nginx -p 8080:80 -v /Users/peiel/nginx/html:/usr/share/nginx/html -v /Users/peiel/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /Users/peiel/nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /Users/peiel/nginx/logs:/var/log/nginx -d nginx
```

### Docker启动RabbitMQ环境

```bash
docker run -d --name uat_rabbitmq -p 5672:5672 -p 15672:15672 -e RABBITMQ_DEFAULT_USER=guest -e RABBITMQ_DEFAULT_PASS=guest rabbitmq:3-management
```