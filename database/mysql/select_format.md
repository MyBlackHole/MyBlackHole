### 查询格式
```sql
总结 

SELECT select_expr [,select_expr,...] [ 
      FROM tb_name++ 
      [WHERE 条件判断] 
      [GROUP BY {col_name | postion} [ASC | DESC], ...] 
      [HAVING WHERE 条件判断] 
      [ORDER BY {col_name|expr|postion} [ASC | DESC], ...] 
      [ LIMIT {[offset,]rowcount | row_count OFFSET offset}] 
] 

完整的 select 语句 
    select distinct * 
    from 表名  
    where .... 
    group by ... having … 
    order by … 
    limit start,count 

执行顺序为： 
    from 表名 
    where .... 
    group by ... 
    select distinct * 
    having ... 
    order by ... 
    limit start,count 

实际使用中，只是语句中某些部分的组合，而不是全部 

```
