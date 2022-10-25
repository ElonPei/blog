---
title: MySQL使用单个语句对分组进行LIMIT操作
date: 2019-04-01
---

# 场景介绍


有一张表，字段为id、姓名，班级、分数，里边记录了所有班级学生的分数，如何使用SQL语句来查询所有班级前三名的学生？


# 思路


1. 对班级进行分组，然后使用 `GROUP_CONCATE` 进行排序并把成绩记录在结果中，当做一个临时表来使用。

2. 查询当前表，INNER JOIN 上边的临时表，使用 FIND_IN_SET 查询所记录的成绩，取1-3即可。


# SQL


```sql
SELECT t.class,
       t.score
FROM `test` AS t
INNER JOIN
  (SELECT i.class,
          GROUP_CONCAT(i.`score`
                       ORDER BY i.`score` DESC) grouped_score
   FROM `test` AS i
   GROUP BY i.class) AS group_max ON t.class = group_max.class
AND FIND_IN_SET(t.`score`, grouped_score) BETWEEN 1 AND 3
ORDER BY t.class,
         t.score DESC;
```


# 思考总结


所有针对GROUP BY结果进行LIMIT操作的情况下都是可以使用这种思路来进行查询的。




end！
