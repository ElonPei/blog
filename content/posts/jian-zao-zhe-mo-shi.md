---
title: 建造者模式
date: 2022-01-03
tags:
  - 设计模式-创建型
---

## 定义

> ☎️ 用户不需要知道内部具体的构建细节。

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

## 拥有角色

Builder：抽象构建者

ConcreteBuilder：具体构建者

Director：指挥者

Product：产品角色

## 类图

![%E5%BB%BA%E9%80%A0%E8%80%85%E6%A8%A1%E5%BC%8F%20fa11b932a8eb44e28f959c155f7328ec/Diagram.drawio.svg](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/建造者模式.drawio.svg)

## 时序图

![%E5%BB%BA%E9%80%A0%E8%80%85%E6%A8%A1%E5%BC%8F%20fa11b932a8eb44e28f959c155f7328ec/Diagram.drawio%201.svg](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/建造者模式%201.svg)

## Director

作用主要有两个

1. 隔离了客户与生产过程。

2. 负责控制产品的生产过程。

客户只需要知道具体构建者的类型。

## 适用环境

需要生产的产品对象具有复杂的对象结构，这些产品对象通常包含多个成员属性。

需要生产的产品对象的属性相互依赖，需要指定其生产顺序。

对象的生产过程独立于创建该对象的类。将创建过程封装在指挥者类中，而不是在建造者类中。

隔离负责对象的创建和使用，并使得相同的创建过程可以创建不同的产品。

## 实际使用场景

一辆汽车对象的生产可以使用建造者模式。

一根笔对象的创建，也可以使用建造者模式。
