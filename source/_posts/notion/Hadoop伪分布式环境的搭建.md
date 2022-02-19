---
categories:
  - 中间件
created_time: February 19, 2022 9:31 AM
date: 2016/06/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Hadoop伪分布式环境的搭建
---


### 一、Win下相关配置工作

1. 安装虚拟机。
2. 导入CentOS虚拟机文件到虚拟机中。
3. 配置CentOS网络适配器->`自定义`>`VMnet8(NAT模式)`。
4. 虚拟机中 `编辑` -> `虚拟网络编辑器` > 选择VMnet8，配置`子网IP`、`子网掩码`和`网关`。

### 二、Linux下相关配置工作

1. 修改主机名 ​
vim /etc/sysconfig/network
    
    NETWORKING=yes HOSTNAME=###
    
2. 配置IP地址 ​
vim /etc/sysconfig/network-scripts/ifcfg-eth0
    
    DEVICE=“eth0” BOOTPROTO=“static” ### HWADDR=“00:0C:29:3C:BF:E7” IPV6INIT=“yes” NM_CONTROLLED=“yes” ONBOOT=“yes” TYPE=“Ethernet” UUID=“ce22eeca-ecde-4536-8cc2-ef0dc36d4a8c” IPADDR=“192.168.1.101” ### NETMASK=“255.255.255.0” ### GATEWAY=“192.168.1.1” ###
    
3. 修改主机名和IP的映射关系
    
    vim /etc/hosts
    
    192.168.1.101
    
4. 关闭防火墙
    
    ```
    #查看防火墙状态
     service iptables status
     #关闭防火墙
     service iptables stop
     #查看防火墙开机启动状态
     chkconfig iptables --list
     #关闭防火墙开机启动
     chkconfig iptables off
    ```
    
5. 安装JDK

解压到恰当目录后，配置JDK环境变量。 vim /etc/profile #在文件最后添加 export JAVA_HOME= *** export PATH=*PATH*:JAVA_HOME/bin

使配置文件生效 `source /***/***` 重启网络服务的命令 `service network restart`

### 安装Hadoop2.4.1

1. 上传hadoop安装包到服务器
    
    注：hadoop2.x 的配置文件`$HADOOP_HOME/etc/hadoop`
    
2. 配置hadoop
    
    第一个：hadoop-env.sh
    
    vim hadoop-env.sh #第27行 export JAVA_HOME=/usr/java/jdk1.7.0_65 第二个：core-site.xml
    
     fs.defaultFS hdfs://weekend-1206-01:9000    hadoop.tmp.dir /home/hadoop/hadoop-2.4.1/tmp  第三个：hdfs-site.xml
    
     dfs.replication 1  第四个：mapred-site.xml
    
    mv mapred-site.xml.template mapred-site.xml vim mapred-site.xml   mapreduce.framework.name yarn  第五个：yarn-site.xml
    
     yarn.resourcemanager.hostname weekend-1206-01    yarn.nodemanager.aux-services mapreduce_shuffle
    
3. 将hadoop添加到环境变量中
4. 格式化namenode（是对namenode进行初始化）
hdfs namenode -format (hadoop namenode -format)
5. 启动hadoop
sbin/start-yarn.sh
    
    先启动HDFS
    
    sbin/start-dfs.sh 再启动YARN
    
6. 验证是否启动成功
    
    使用jps命令验证
    
    23615 NameNode 16235 Jps ….. SecondaryNameNode ….. NodeManager ….. ResourceManager ….. DataNode
    
    HDFS管理界面： `http://hostname:50070
    
    MR管理界面： `http://hostname:8080 ​