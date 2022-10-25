---
title: C Sharp 中 JSON 字符串和对象之间转换时发生错误
date: 2015-04-19
tags:
  - C
---

## 问题描述

```
错误1 命名空间“Newtonsoft.Json”中不存在类型或命名空间名称“JavaScriptConvert”(是缺少程序集引用吗?)    
    d:\***\***\Visual Studio 2005\Projects\***\***\***.cs
    732    36    ***
```

## 解决办法

1. `Newtonsoft.Json` 包引用没问题 `./c#备忘/Newtonsoft.Json.dll`

2. **最终原因**：`Newtonsoft.Json` 库由于新旧版本的差异，方法的名字发生了变化，如下

```
1.JavaScriptArray    --->        JArray
2.JavaScriptConvert    --->        JsonConvert
3.JavaScriptObject    --->        JObject
```

## 正确的代码

```c#
// 从一个对象信息生成Json串
public static string ObjectToJson(object obj){
  	return Newtonsoft.Json.JsonConvert.SerializeObject(obj);
}
//从一个Json串生成对象信息
public static object JsonToObject(string jsonString, object obj){
  	return Newtonsoft.Json.JsonConvert.DeserializeObject(jsonString, obj.GetType());
}
```
