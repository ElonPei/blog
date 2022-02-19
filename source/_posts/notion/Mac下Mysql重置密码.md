---
categories:
  - Mac
created_time: February 19, 2022 9:30 AM
date: 2018/08/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Mac下Mysql重置密码
---


# step1.关闭mysql服务

方式一：直接杀进程

```
ps -ef | grep mysqld
kill -9 [pid]
```

方式二：设置 > MySql > Stop MySql Server

# step2.进入终端执行以下命令

```
cd /usr/local/mysql/bin/
sudo su
./mysqld_safe --skip-grant-tables &
```

ps: 此时、mysql已经启动

# step3.输入命令进入mysql交互环境

```
./mysql
```

# step4.在mysql交互环境重置密码

```
 FLUSH PRIVILEGES;
 SET PASSWORD FOR 'root'@'localhost' = PASSWORD('你的新密码');
```

end!