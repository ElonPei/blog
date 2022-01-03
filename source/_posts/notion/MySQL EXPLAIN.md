---
categories:
  - 数据库
date: 2022/01/03
excerpt: 熟练使用 EXPLAIN 我们可以分析SQL查询语句的性能和发现优化思路。
show_category: Yes
status: 待发布
tags:
  - MySQL
title: MySQL EXPLAIN
---


- SQL语句
    
    ```sql
    explain
    SELECT COUNT(0)
    FROM (SELECT us.user_id,
                 us.user_name,
                 us.user_code,
                 dept.dept_id,
                 dept.dept_code,
                 dept.dept_name,
                 dept.dept_path,
                 dept.path,
                 post.post_id,
                 post.post_code,
                 post.post_name,
                 MAX(IFNULL(app_dept.path, '.D0001')) appl_path
          FROM mdm_prod.mdm_user us
                   INNER JOIN mdm_prod.mdm_dept dept ON dept.dept_code = us.dept_code AND dept.is_del = 0
                   LEFT JOIN mdm_prod.mdm_post post ON us.post_code = post.post_code AND post.is_del = 0
                   LEFT JOIN (SELECT app_user.*, dept.path
                              FROM hr_attendance_shift_appl_dept_new app_user
                                       INNER JOIN mdm_prod.mdm_dept dept
                                                  ON app_user.dept_code = dept.dept_code AND dept.is_del = 0 AND
                                                     dept.`status` = 1) app_dept
                             ON dept.path LIKE CONCAT(app_dept.path, '%')
          WHERE us.is_del = 0
            AND us.`status` = 1
            AND dept.path LIKE CONCAT('.D0001', '%')
          GROUP BY us.user_code) table_count
    ```
    

## 输出结果

![](/notion_images/e761d5da997532d3309f3246d6dced43.png)

## 各个结果解析

- id
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> select 查询的标识符，每个 select 都会自动分配一个唯一的标识符。
    
    </aside>
    
- select_type
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> select 查询的类型
    
    </aside>
    
    - SIMPLE： 表示不包含UNION查询或子查询
    - PRIMARY： 表示次查询是最外层的查询
    - UNION： 表示此查询是UNION的第二或者随后的查询
    - DEPENDENT UNION：UNION 中的第二个或者后面的查询语句，取决于后面的查询。
    - SUBQUERY： 子查询中的第一个 SELECT
    - DEPENDENT SUBQUERY： 子查询中的第一个SELECT，取决于外面的查询，即子查询依赖与外层的查询结果。
    - DERIVED：派生表的 SELECT，FROM子句的子查询
    - UNCACHEABLE SUBQUERY：一个子查询的结果不能被缓存，必须重新评估外链接的第一行
- table
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 查询的是哪个表
    
    </aside>
    
- type
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> join 类型，表示MySQL在表中找到所需行的方式，又称为“访问类型”。
    
    </aside>
    
    all → index → range → ref → eq_ref → const → system → NULL  性能 差 → 好
    
    - ALL：Full table Scan
    - index：Full index Scan
    - range：只索引给定范围的行
    - ref：表示上述表的链接匹配条件，即那些列或常量用户查询索引列上的值。
    - eq_ref：类似ref，区别是使用的索引是唯一索引，多表链接中，使用了primary key 或 unique key 作为关联条件
    - const、system：索引命中，MySQL针对查询转换为一个常量，system表示只有一行命中索引。
    - NULL：MySQL在优化过程中，无需回标操作，直接通过索引就能返回结果。
- possible_keys
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 此次查询中可能选用的索引
    
    </aside>
    
- key
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 此次查询中确切使用到的索引
    
    </aside>
    
- key_len
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 索引字段最大可能的长度，单位是字节。
    
    </aside>
    
    不损失精度的情况下，长度越短越好。
    
- ref
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 哪个字段或常数与 key 一起被使用
    
    </aside>
    
- rows
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 表示一共有多少行被扫描了，这个行数是一个估算值。
    
    </aside>
    
- Extra
    
    <aside>

    <img class="emoji" draggable="false" alt="💡" src="https://twemoji.maxcdn.com/v/13.1.0/72x72/1f4a1.png"/> 额外的信息
    
    </aside>
    
    - Using filesort：表示MySQL进行了额外的排序操作
    - Using index：覆盖索引扫描，说明性能不错
    - Using temporary：查询是使用了临时表，一般出现在排序、分组、联表的情况，查询效率不高，建议优化。
    - Using Where
    - Using join buffer
    - Impossible where
    - Select tales optimized away

## 其他注意事项

- 不会显示 触发器、存储过程的信息和用户自定义的函数
- 不考虑各种cache
- 不能显示MySQL在查询是做的优化工作
- 部分信息是估算的，并非精确的值
- 只能解释select查询