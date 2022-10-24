---
title: Mac 下 MySQL 重置密码
date: 2018-08-01
tags:
  - MySQL
  - Mac
---

## 1. 关闭 MySQL 服务

方式一：直接杀进程

  ```
  ps -ef | grep mysqld
  kill -9 [pid]
```

方式二：设置 > MySql > Stop MySql Server

## 2. 进入终端执行以下命令

 ```
 cd /usr/local/mysql/bin/
 sudo su
 ./mysqld_safe --skip-grant-tables &
```

Tips: 此时、MySQL 已经启动

## 3. 输入命令进入 MySQL 交互环境

 ```
 ./mysql
```

## 4. 在 MySQL 交互环境重置密码

 ```
  FLUSH PRIVILEGES;
  SET PASSWORD FOR 'root'@'localhost' = PASSWORD('你的新密码');
```

end!
