---
categories:
  - Mac
date: 2022/01/16
excerpt: 作为Java程序员，在M1芯片的Mac平台工作已经接近两年，近期购入M1Pro的MacBook
  Pro后，简单折腾和优化了下开发环境，并记录本文章，供后续查看。
show_category: Yes
status: 待发布
tags:
  - Mac
title: 基于M1芯片的Mac环境搭建记录
---


<aside>

<img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 作为Java程序员，在M1芯片的Mac平台工作已经接近两年，近期购入M1Pro的MacBook Pro后，简单折腾和优化了下开发环境，并记录本文章，供后续查看。
</aside>

# Docker启动Nacos环境

```bash
# 1. 拉取镜像
docker pull zhusaidong/nacos-server-m1:2.0.3
# 2. 启动服务
docker run --name nacos-standalone -e MODE=standalone -e JVM_XMS=512m -e JVM_XMX=512m -e JVM_XMN=256m -p 8848:8848 -d zhusaidong/nacos-server-m1:2.0.3
```

# Docker启动Nginx环境

```bash
docker run --name my-nginx -p 8080:80 -v /Users/peiel/nginx/html:/usr/share/nginx/html -v /Users/peiel/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /Users/peiel/nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /Users/peiel/nginx/logs:/var/log/nginx -d nginx
```

# Docker启动mariadb环境

```bash
docker run --name my-mariadb -e MARIADB_ROOT_PASSWORD=q1w2e3r4 -p 3306:3306 -d mariadb:latest
```