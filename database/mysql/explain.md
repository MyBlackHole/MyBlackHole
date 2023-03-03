### explain
- id: 选择标识符 
- select_type: 查询类型 
- table: 输出结果集的表 
- partitions: 匹配的分区 
- type: 表的连接类型 
- possible_keys: 查询时可能使用的索引 
- key: 实际使用的索引 
- key_len: 索引字段的长度 
- ref: 列与索引的比较 
- rows: 扫描出的行数 
- filtered: 按表条件过滤的行百分比 
- extra: 执行情况描述和说明 

1. select_type
SIMPLE: 简单SELECT(不使用UNION或子查询) 
PRIMARY: 最外面的SELECT 
UNION: UNION中的第二个或后面的SELECT语句 
DEPENDENT UNION: 子查询中的UNION中的第二个或后面的SELECT语句 
UNION RESULT: UNION 的结果 
SUBQUERY: 子查询中的第一个SELECT，子查询里没用UNION 
DEPENDENT SUBQUERY: 子查询中的第一个SELECT 
DERIVED: 导出表的SELECT(FROM子句的子查询) 

2. type
const: 
    单表查询：根据主键或唯一索引，查出一条唯一数据记录 
    多表联合查询：根据主键或唯一索引，查出一条唯一数据记录 
eq_ref: 
    多表联合查询：关联的字段都是各自表的主键。 
ref: 
    单表查询：使用唯一索引的前缀索引、或非唯一索引的前缀索引、或非唯一索引的全列索引进行查询。 
    多表联合查询：关联的字段不是自己表的主键，是外键。 
range: 
    单表查询：使用一个单列索引，或复合索引的前缀索引进行范围查询； 
    多表联合查询：使用一个单列索引，或复合索引的前缀索引进行范围查询。 
index: 
    无Where语句：select后的列都是同一个索引里面的列 
    有Where语句：where后面是索引的非前缀索引列 
ALL: 
    无Where语句： select后面有不是索引的列 
    有Where语句：where后面有不是索引的列 

3. extra
Usingindexcondition： 
    必带where语句，辅助索引扫描加回表 
Usingindex： 
    无where语句：select后只跟主键 
    有where语句：select后只跟主键，where后跟主键查询 
Using where;Using index： 
    必带where语句，且直接从辅助索引获取全部数据，即辅助索引的覆盖扫描 
Usingwhere： 
    必带where语句，且where语句用不上索引 
Usingfilesort： 
    使用非索引列进行order by 导致的排序，效率偏低，需要优化。 

    优化手段包括： 
    修改业务逻辑，不用SQL语句中进行order by 
    使用索引列进行order by 

Usingtemporary: 
    多表关联left join时，on后面使用非直接关联会使用临时表。 

    优化手段包括： 
    修改业务逻辑，非直接关联转成直接关联 
    将非直接关联转为子查询 
