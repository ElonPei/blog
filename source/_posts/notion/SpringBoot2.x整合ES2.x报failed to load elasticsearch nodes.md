---
categories:
  - Java
created_time: February 19, 2022 9:30 AM
date: 2018/06/30
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: SpringBoot2.x整合ES2.x报failed to load elasticsearch nodes
---


## 在SpringBoot整合ES时，遇到如下错误。

```
ERROR 2075 --- [           main] .d.e.r.s.AbstractElasticsearchRepository : failed to load elasticsearch nodes : org.elasticsearch.client.transport.NoNodeAvailableException: None of the configured nodes are available:
```

## 相关配置

`pom.xml`部分配置

```
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
    </dependency>

```

`application.properties` ES相关配置

```
spring.data.elasticsearch.repositories.enabled=true
spring.data.elasticsearch.cluster-nodes=192.168.xxx.xxx\:9300
```

## 版本

- SpringBoot2.0
- ES2.4

## 问题原因

导致一直解决不了问题的原因是[这个链接](https://github.com/spring-projects/spring-data-elasticsearch/wiki/Spring-Data-Elasticsearch---Spring-Boot---version-matrix)显示SpringBoot2.0是兼容ES2.4的，所以我首先排除了兼容问题。

网上查找，报这个错的原因是因为IP限制，无论是修改ES的配置中`network.host`属性为`0.0.0.0`也好，我链接ES的机器的内网IP也好，**都不好使**。

之后看到了[这篇文章](https://blog.csdn.net/lusyoe/article/details/80107865)，我再次怀疑有可能版本兼容的问题，修改SpringBoot为`1.5.9.RELEASE`版本，不报错了。

这个兼容性问题，要么降级SpringBoot，要么升级ES，还是更新ES比较靠谱。

---

> 降级是不能降级的，这辈子都不可能降级
>