---
title: Nginx 添加用户鉴权
date: 2019-05-31
tags:
  - Nginx
---

## Nginx 添加用户鉴权

### 1. 添加依赖

```
yum -y install httpd-tools
```

### 2. 配置 Nginx

```lua
location / {
  ...        
  auth_basic "secret";        
  auth_basic_user_file /usr/local/nginx/db/passwd.db;        
  ...
}
```

❗**记得nginx下创建目录**

### 3. 创建用户及密码

```
htpasswd -c /usr/local/nginx/db/passwd.db usrname
```

然后根据提示输入密码，重启nginx就可以了。
