---
title: 简单工厂模式
date: 2021-12-12
---

## 包含角色

Factory： 工厂角色

Prodect：抽象产品角色

ConcreteProdect：具体产品角色

### 类图


![https://raw.githubusercontent.com/peiel/oss/master/uPic/OFpNou.jpg](https://raw.githubusercontent.com/peiel/oss/master/uPic/OFpNou.jpg)

### 时序图


![https://raw.githubusercontent.com/peiel/oss/master/uPic/n099B0.jpg](https://raw.githubusercontent.com/peiel/oss/master/uPic/n099B0.jpg)

## 模式要点

工厂方法是静态方法

最大的问题是工厂方法职责过重，增加新的产品必须修改工厂类的判断逻辑，违背“开闭原则”

## 使用场景

工厂类需要创建的对象种类比较少

## 实际应用场景举例

java.text.DateFormat.getDateInstance(style)

KeyGenerator.getInstance(“DESede”)
