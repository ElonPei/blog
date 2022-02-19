---
categories:
  - C#
created_time: February 19, 2022 9:31 AM
date: 2015/04/19
excerpt: 报错解决
show_category: Yes
status: 待发布
tags:
  - C#
updated_time: February 19, 2022 11:12 AM
title: C#中JSON字符串和对象之间转换时发生错误
---


> 报错
> 

```
错误1 命名空间“Newtonsoft.Json”中不存在类型或命名空间名称“JavaScriptConvert”(是缺少程序集引用吗?)
d:\***\***\Visual Studio 2005\Projects\***\***\***.cs
732    36    ***
```

> 解决办法
> 
1. Newtonsoft.Json 包引用没问题 `./c#备忘/Newtonsoft.Json.dll`
2. **最终原因：**Newtonsoft.Json库由于新旧版本的差异，方法的名字发生了变化，如下：
    
    `1.JavaScriptArray    --->        JArray` `2.JavaScriptConvert    --->        JsonConvert` `3.JavaScriptObject    --->        JObject`
    

> 正确代码
> 

```
// 从一个对象信息生成Json串
public static string ObjectToJson(object obj){
    return Newtonsoft.Json.JsonConvert.SerializeObject(obj);
}
//从一个Json串生成对象信息
public static object JsonToObject(string jsonString, object obj){
    return Newtonsoft.Json.JsonConvert.DeserializeObject(jsonString, obj.GetType());
}
```