---
title: 《Elasticsearch权威指南》读书笔记
date: 2018-03-06
tags:
  - ES
---

> 本文为《Elasticsearch权威指南》阅读笔记，原书链接请点击这里查看。

# **索引(动词)操作与索引(名词)**


整个`PUT`操作的动作称之为索引(动词)


```
 PUT /megacorp/employee/1
 {
   "first_name" : "John",
   "last_name" :  "Smith",
   "age" :        25,
   "about" :      "I love to go rock climbing",
   "interests": [ "sports", "music" ]
 }
```

`megacorp` 索引名称

`employee` 索引类型

`1` 特定雇员的id

# **检索文档**


```
 GET /megacorp/employee/1
```


```
 {
 "_index" :   "megacorp",
 "_type" :    "employee",
 "_id" :      "1",
 "_version" : 1,
 "found" :    true,
 "_source" :  {
     "first_name" :  "John",
     "last_name" :   "Smith",
     "age" :         25,
     "about" :       "I love to go rock climbing",
     "interests":  [ "sports", "music" ]
 }
 }
```

# **轻量搜索**

## **搜索所有雇员**


```
 GET /megacorp/employee/_search
```

## **搜索last_name为Smith的所有雇员**


```
 GET /megacorp/employee/_search?q=last_name:Smith
```

# **使用查询表达式搜索**


**搜索last_name为Smith的所有雇员**可以改写为以下形式


```
 GET /megacorp/employee/_search
 {
   "query" : {
       "match" : {
           "last_name" : "Smith"
       }
   }
 }
```

# **更复杂的搜索**

## **过滤器 filter 的使用**


搜索last_name为Smith的所有雇员，并只需要年龄大于30的。


```
 GET /megacorp/employee/_search
 {
   "query" : {
       "bool": {
           "must": {
               "match" : {
                   "last_name" : "smith"
               }
           },
           "filter": {
               "range" : {
                   "age" : { "gt" : 30 }
               }
           }
       }
   }
 }
```

# **全文搜索**


搜索下所有喜欢攀岩（rock climbing）的雇员


```
 GET /megacorp/employee/_search
 {
   "query" : {
       "match" : {
           "about" : "rock climbing"
       }
   }
 }
```


结果：


```
 {
  ...
  "hits": {
     "total":      2,
     "max_score":  0.16273327,
     "hits": [
        {
           ...
           "_score":         0.16273327,
           "_source": {
              ...
              "about":       "I love to go rock climbing",
              ...
           }
        },
        {
           ...
           "_score":         0.016878016,
           "_source": {
              ...
              "about":       "I love to go rock albums",
              ...
           }
        }
     ]
  }
 }
```


`_score` 为相关性得分字段，得分越高，排名越靠前。

# **短语搜索**


仅匹配同时包含 “rock” 和 “climbing” ，并且 二者以短语 “rock climbing” 的形式紧挨着的雇员记录。


```
 GET /megacorp/employee/_search
 {
   "query" : {
       "match_phrase" : {
           "about" : "rock climbing"
       }
   }
 }
```

# **高亮搜索**


ES中使用`highlight`参数使结果高亮。


```
 GET /megacorp/employee/_search
 {
   "query" : {
       "match_phrase" : {
           "about" : "rock climbing"
       }
   },
   "highlight": {
       "fields" : {
           "about" : {}
       }
   }
 }
```

# **分析-聚合搜索(aggregations)**


挖掘出雇员中最受欢迎的兴趣爱好


```
 GET /megacorp/employee/_search
 {
 "aggs": {
   "all_interests": {
     "terms": { "field": "interests" }
   }
 }
 }
```
