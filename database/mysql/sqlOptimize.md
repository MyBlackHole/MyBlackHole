### SQL优化
1. 尽量避免在字段开头模糊查询,会导致数据库引擎放弃索引进行全表扫描
```sql
# 例如
SELECT * FROM t WHERE username LIKE '%陈%'

优化方式：尽量在字段后面使用模糊查询
SELECT * FROM t WHERE username LIKE '陈%'

```
如果需求是要在前面使用模糊查询
    使用MySQL内置函数INSTR(str,substr) 来匹配,作用类似于java中的indexOf(),查询字符串出现的角标位置
    使用FullText全文索引,用match against 检索
    数据量较大的情况,建议引用ElasticSearch、solr,亿级数据量检索速度秒级
    当表数据量较少（几千条儿那种）,别整花里胡哨的,直接用like '%xx%'

2. 尽量避免使用in 和not in,会导致引擎走全表扫描
```sql
# 例如
SELECT * FROM t WHERE id IN (2,3)
优化方式：如果是连续数值,可以用between代替。如下

如果是子查询,可以用exists代替
# 不走索引
select * from A where A.id in (select id from B);
# 走索引
select * from A where exists (select * from B where B.id = A.id);
```

3. 尽量避免使用 or,会导致数据库引擎放弃索引进行全表扫描
```sql
# 例如
SELECT * FROM t WHER Eid = 1 OR id = 3

优化方式：可以用union代替or。如下
SELECT * FROM t WHERE id = 1
   UNION
SELECT * FROM t WHERE id = 3
```

4. 尽量避免进行null值的判断,会导致数据库引擎放弃索引进行全表扫描
```sql
# 例如
SELECT * FROM t WHERE score IS NULL

优化方式：可以给字段添加默认值0,对0值进行判断。如下：
SELECT * FROM t WHERE score = 0
```

5. 尽量避免在where条件中等号的左侧进行表达式、函数操作,会导致数据库引擎放弃索引进行全表扫描
```sql
# 例如
# 全表扫描
SELECT * FROM T WHERE score/10 = 9
# 走索引
SELECT * FROM T WHERE score = 10*9
```

6. 当数据量大时,避免使用where 1=1的条件。通常为了方便拼装查询条件,我们会默认使用该条件,数据库引擎会放弃索引进行全表扫描
```sql
# 例如
SELECT username, age, sex FROM T WHERE 1=1
优化方式：用代码拼装sql时进行判断,没 where 条件就去掉 where,有where条件就加 and
```

7. 查询条件不能用 <> 或者 !=[^1]
使用索引列作为条件进行查询时，需要避免使用<>或者!=等判断条件。如确实业务需要，使用到不等于符号，需要在重新评估索引建立，避免在此字段上建立索引，改由查询条件中其他索引字段代替

8. where条件仅包含复合索引非前置列
```sql
如下:复合(联合)索引包含key_part1，key_part2，key_part3三列，但SQL语句没有包含索引前置列"key_part1"，按照MySQL联合索引的最左匹配原则，不会走联合索引

select col1 from table where key_part2=1 and key_part3=2
```

9. 隐式类型转换造成不使用索引
```sql
如下:SQL语句由于索引对列类型为varchar，但给定的值为数值，涉及隐式类型转换，造成不能正确走索引

select col1 from table where col_varchar=123; 
```

10. order by 条件要与where中条件一致，否则order by不会利用索引进行排序
```sql
-- 不走age索引
SELECT * FROM t order by age;

-- 走age索引
SELECT * FROM t where age > 0 order by age;


对于上面的语句，数据库的处理顺序是：

第一步：根据where条件和统计信息生成执行计划，得到数据。
第二步：将得到的数据排序。当执行处理数据（order by）时，数据库会先查看第一步的执行计划，看order by 的字段是否在执行计划中利用了索引。如果是，则可以利用索引顺序而直接取得已经排好序的数据。如果不是，则重新进行排序操作。
第三步：返回排序后的数据。
当order by 中的字段出现在where条件中时，才会利用索引而不再二次排序，更准确的说，order by 中的字段在执行计划中利用了索引时，不用排序操作。

这个结论不仅对order by有效，对其他需要排序的操作也有效。比如group by 、union 、distinct等
```

11. 正确使用hint优化语句
```sql
MySQL中可以使用hint指定优化器在执行时选择或忽略特定的索引。一般而言，处于版本变更带来的表结构索引变化，更建议避免使用hint，而是通过Analyze table多收集统计信息。但在特定场合下，指定hint可以排除其他索引干扰而指定更优的执行计划。

USE INDEX 在你查询语句中表名的后面，添加 USE INDEX 来提供希望 MySQL 去参考的索引列表，就可以让 MySQL 不再考虑其他可用的索引。例子: SELECT col1 FROM table USE INDEX (mod_time, name)...
IGNORE INDEX 如果只是单纯的想让 MySQL 忽略一个或者多个索引，可以使用 IGNORE INDEX 作为 Hint。例子: SELECT col1 FROM table IGNORE INDEX (priority) ...
FORCE INDEX 为强制 MySQL 使用一个特定的索引，可在查询中使用FORCE INDEX 作为Hint。例子: SELECT col1 FROM table FORCE INDEX (mod_time) ...
在查询的时候，数据库系统会自动分析查询语句，并选择一个最合适的索引。但是很多时候，数据库系统的查询优化器并不一定总是能使用最优索引。如果我们知道如何选择索引，可以使用FORCE INDEX强制查询使用指定的索引。

例如：

SELECT * FROM students FORCE INDEX (idx_class_id) WHERE class_id = 1 ORDER BY id DESC;
```

### SELECT其他优化
1. 避免出现select *
首先，select * 操作在任何类型数据库中都不是一个好的SQL编写习惯。
使用select * 取出全部列，会让优化器无法完成索引覆盖扫描这类优化，会影响优化器对执行计划的选择，也会增加网络带宽消耗，更会带来额外的I/O,内存和CPU消耗。
建议提出业务实际需要的列数，将指定列名以取代select *。

2. 避免出现不确定结果的函数
特定针对主从复制这类业务场景。由于原理上从库复制的是主库执行的语句，使用如now()、rand()、sysdate()、current_user()等不确定结果的函数很容易导致主库与从库相应的数据不一致。另外不确定值的函数,产生的SQL语句无法利用query cache。


3. 多表关联查询时，小表在前，大表在后。
在MySQL中，执行 from 后的表关联查询是从左往右执行的（Oracle相反），第一张表会涉及到全表扫描，所以将小表放在前面，先扫小表，扫描快效率较高，在扫描后面的大表，或许只扫描大表的前100行就符合返回条件并return了。
一般都是通过索引字段进行表关联，应保证关联大表的索引且确保大表在右，这样可以利用到大表索引

例如：表1有50条数据，表2有30亿条数据；如果全表扫描表2，你品，那就先去吃个饭再说吧是吧

4. 使用表的别名

当在SQL语句中连接多个表时，请使用表的别名并把别名前缀于每个列名上。这样就可以减少解析的时间并减少哪些友列名歧义引起的语法错误。


5. 用where字句替换HAVING字句

避免使用HAVING字句，因为HAVING只会在检索出所有记录之后才对结果集进行过滤，而where则是在聚合前刷选记录，如果能通过where字句限制记录的数目，那就能减少这方面的开销。HAVING中的条件一般用于聚合函数的过滤，除此之外，应该将条件写在where字句中。

where和having的区别：where后面不能使用组函数

6.调整Where字句中的连接顺序

MySQL采用从左往右，自上而下的顺序解析where子句。根据这个原理，应将过滤数据多的条件往前放，最快速度缩小结果集

#### 增删改 DML[[../sqlClassify#DML]] 语句优化



[^1]: 进行范围查询（比如>、< 、>=、<=、in等条件）时往往会出现上述情况,而上面提到的临界值根据场景不同也会有所不同
