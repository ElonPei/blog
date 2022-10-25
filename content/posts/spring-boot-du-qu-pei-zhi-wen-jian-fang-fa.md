---
title: Spring Boot 读取配置文件方法
date: 2018-06-21
tags:
  - Java
---

## 1. 使用@Value方式读取，示例如下

在 `application.properties` 中加入如下内容

```
test=123
```

然后在类中可以使用 `@Value` 注解取值

```java
@Value("${test}")
private String test;
```

## 2. 使用Environment方式取值

同样，以读取方式一中的配置文件内容为例

```java
@Autowired
private Environment env;

@RequestMapping("/test")
public String test(){
    return env.getProperty("test");
}
```

## 3. 自定义配置文件映射

在 `application.properties` 文件加入内容如下。

```
test.name=zhangsan
test.age=1
```

新建实体 `Test.java`，内容如下。

```java
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;

@Data
@ConfigurationProperties(prefix = "test")
public class Test {
    private String name;
    private int age;
}

```

可以使用 `@PropertySource("classpath:my2.properties")` 指定读取的配置文件。
