---
categories:
  - 中间件
created_time: February 19, 2022 9:31 AM
date: 2018/03/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: ElasticSearch聚合搜索报错解决
---


## 请求语句

```
curl -XGET 'localhost:9200/megacorp/employee/_search?pretty' -H 'Content-Type: application/json' -d'{  "aggs": {    "all_interests": {      "terms": { "field": "interests" }    }  }}
```

## 报错

```
{  "error" : {    "root_cause" : [      {        "type" : "illegal_argument_exception",        "reason" : "Fielddata is disabled on text fields by default. Set fielddata=true on [interests] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."      }    ],    "type" : "search_phase_execution_exception",    "reason" : "all shards failed",    "phase" : "query",    "grouped" : true,    "failed_shards" : [      {        "shard" : 0,        "index" : "megacorp",        "node" : "O31wTokAT2WghaK8xUE_hg",        "reason" : {          "type" : "illegal_argument_exception",          "reason" : "Fielddata is disabled on text fields by default. Set fielddata=true on [interests] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."        }      }    ]  },  "status" : 400}
```

## 解决办法

根据错误日志，根据`reason`字段，可以得知将字段`interests`的`fielddata`属性设置为`true`即可。

```
PUT /megacorp/_mapping/employee{  "properties": {    "interests": {      "type":"text",      "fielddata" : true    }  }}
```

转载请注明出处： www.peierlong.com