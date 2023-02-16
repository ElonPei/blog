---
title: 基于 M1 芯片的 macOS 环境搭建记录
date: 2022-01-16
tags:
  - macOS
  - M1
---


> 💡 作为程序员，在 M1 芯片的 Mac 平台工作已经接近两年，近期购入 M1 Pro 的 MacBook Pro 后，简单折腾和优化了下开发环境，并记录本文章，供后续查看。

## 常用软件安装

### iTem2

Mac 上比较好用的终端工具

支持 m1

### Anaconda

`Python` 环境管理，官方已经原生支持 m1，使用官方下载即可。

支持 `m1`

## 开发环境

### Homebrew

支持 m1

```bash
# 安装命令
/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"
```

### Docker

支持 m1

```sh
brew install --cask docker
```

### brew 安装 MySQL 环境

支持 m1

```bash
brew install mysql
```

### brew 安装 Redis 环境

支持 m1

```bash
brew install redis
```

### Pytnon 环境及常用依赖安装

安装 conda 环境后，我们就可以管理我们的 python 环境了。

```bash
# 安装pandas
conda install pandas
# 安装numpy
conda install numpy
# 安装rich
conda install rich
# 安装mysql支持
conda install PyMySQL
# 安装jupyter
conda install jupyter
```

### Docker 启动 Nacos 环境

```bash
# 1. 拉取镜像
docker pull zhusaidong/nacos-server-m1:2.0.3
# 2. 启动服务
docker run --name nacos-standalone -e MODE=standalone -e JVM_XMS=512m -e JVM_XMX=512m -e JVM_XMN=256m -p 8848:8848 -d zhusaidong/nacos-server-m1:2.0.3
```

### Docker 启动 Nginx 环境

```bash
docker run --name my-nginx -p 8080:80 -v /Users/peiel/nginx/html:/usr/share/nginx/html -v /Users/peiel/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /Users/peiel/nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /Users/peiel/nginx/logs:/var/log/nginx -d nginx
```

### Docker 启动 RabbitMQ 环境

```bash
docker run -d --name uat_rabbitmq -p 5672:5672 -p 15672:15672 -e RABBITMQ_DEFAULT_USER=guest -e RABBITMQ_DEFAULT_PASS=guest rabbitmq:3-management
```

TODO Docker 启动 Seata 环境
