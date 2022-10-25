---
title: CentOS系统环境下安装MongoDB
date: 2018-01-01
---

1. 根据自己的实际系统环境，下载所要的文件

2. 然后解压：

```
tar zxvf mongodb-linux-x86_64-2.2.3.tgz
```

3. 移动目录到/usr/local/mongodb

```
mv mongodb-linux-x86_64-2.2.3 /usr/local/mongodb
```


<!--more-->



4. 进入mongodb目录

```
cd /usr/local/mongodb
```

5. 新建自定义数据目录

```
mkdir -p ./data/db/
```

6. 新建日志目录

```
mkdir logs
```

7. 以后台运行方式启动mongodb

```
/usr/local/mongodb/bin/mongod --dbpath=/usr/local/mongodb/data/db --logpath=/usr/local/mongodb/logs/mongodb.log --fork
```

8. 设置开机自启动：

```
echo "/usr/local/mongodb/bin/mongod --dbpath=/usr/local/mongodb/data/db --logpath=/usr/local/mongodb/logs/mongodb.log --fork" >> /etc/rc.local
```

查看MongoDB日志

```
tail -f /usr/local/mongodb/logs/mongodb.log
```


## 参数解释: 

`--dbpath` 数据库路径(数据文件)

`--logpath` 日志文件路径

`--master` 指定为主机器

`--slave` 指定为从机器

`--source` 指定主机器的IP地址

`--pologSize` 指定日志文件大小不超过64M.因为resync是非常操作量大且耗时，最好通过设置一个足够大的oplogSize来避免resync(默认的 oplog大小是空闲磁盘大小的5%)。

`--logappend` 日志文件末尾添加

`--port` 启用端口号

`--fork` 在后台运行

`--only` 指定只复制哪一个数据库

`--slavedelay` 指从复制检测的时间间隔

`--auth` 是否需要验证权限登录(用户名和密码)
