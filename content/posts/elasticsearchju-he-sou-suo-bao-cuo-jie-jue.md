---
title: ElasticSearch聚合搜索报错解决
date: 2018-03-01
---

## 请求语句


`json
rl -XGET 'localhost:9200/megacorp/employee/_search?pretty' -H 'Content-Type: application/json' -d'
"aggs": {
  "all_interests": {
    "terms": { "field": "interests" }
  }
}
```


<!-- more -->


## 报错


`json
"error" : {
  "root_cause" : [
    {
      "type" : "illegal_argument_exception",
      "reason" : "Fielddata is disabled on text fields by default. Set fielddata=true on [interests] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
    }
  ],
  "type" : "search_phase_execution_exception",
  "reason" : "all shards failed",
  "phase" : "query",
  "grouped" : true,
  "failed_shards" : [
    {
      "shard" : 0,
      "index" : "megacorp",
      "node" : "O31wTokAT2WghaK8xUE_hg",
      "reason" : {
        "type" : "illegal_argument_exception",
        "reason" : "Fielddata is disabled on text fields by default. Set fielddata=true on [interests] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
      }
    }
  ]
},
"status" : 400
```


## 解决办法


根据错误日志，根据`reason`字段，可以得知将字段`interests`的`fielddata`属性设置为`true`即可。


`json
T /megacorp/_mapping/employee
"properties": {
  "interests": {
    "type":"text",
    "fielddata" : true
  }
}
```




转载请注明出处： www.peierlong.com
