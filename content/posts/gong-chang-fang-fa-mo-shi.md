---
title: 工厂方法模式
date: 2021-12-18
tags:
  - 设计模式-创建型
---

# 工厂方法模式


categories: 设计模式

created_time: September 7, 2022 7:41 PM

date: 2021/12/18

excerpt: 工厂方法模式

show_category: Yes

status: 已发布

tags: 设计模式-创建型

updated_time: October 20, 2022 3:08 PM


> 工厂方法模式又称为工厂模式

> 


## 包含角色


Product：抽象产品

ConcreteProduct：具体产品

Factory：抽象工厂

ConcreteFactory：具体工厂


### 类图


![https://raw.githubusercontent.com/peiel/oss/master/uPic/nxX7wM.jpg](https://raw.githubusercontent.com/peiel/oss/master/uPic/nxX7wM.jpg)


### 时序图


![https://raw.githubusercontent.com/peiel/oss/master/uPic/DhIrvA.jpg](https://raw.githubusercontent.com/peiel/oss/master/uPic/DhIrvA.jpg)


## 模式要点


工厂方法模式是简单工厂模式更进一步的抽象，抽象工厂中声明了工厂的方法，用于返回一个产品，这是工厂方法模式的核心。

对于用户来说，只需要关心使用那个具体工厂，不需要关心产品

添加实现只需要添加具体工厂和具体实现，拓展性好，符合开闭原则。


## 适用环境


将创建对象的任务委托给多个工厂子类的某一个


## 实际应用场景举例


JDBC的工厂方法


`java
nnection conn=DriverManager.getConnection("jdbc:microsoft:sqlserver://localhost:1433; DatabaseName=DB;user=sa;password=");
atement statement=conn.createStatement();
sultSet rs=statement.executeQuery("select * from UserInfo");
```
