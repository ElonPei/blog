---
categories:
  - Nginx
created_time: November 23, 2021 10:44 PM
date: 2019/05/31
excerpt: 对于Nginx需要临时添加简单认证，可以使用该方法。
show_category: Yes
status: 待发布
tags:
  - Nginx
updated_time: February 20, 2022 9:38 AM
title: Nginx 添加用户鉴权
---


# Nginx 添加用户鉴权

## 1. 添加依赖

```
yum -y install httpd-tools
```

## 2. 配置nginx

```lua
location / {
...        
auth_basic "secret";        
auth_basic_user_file /usr/local/nginx/db/passwd.db;        
...
}
```

<aside>

<img class="emoji" draggable="false" alt="❗" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/2757.png"/> **记得nginx下创建目录**
</aside>

## 3. 创建用户及密码

```
htpasswd -c /usr/local/nginx/db/passwd.db usrname
```

然后根据提示输入密码，重启nginx就可以了。