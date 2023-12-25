# sql

|**函数名**|**返回类型**|**描述**|
|---|---|---|
|pg_column_size(any)|int|存储一个指定的数值需要的字节数（可能压缩过）|
|pg_database_size(oid)|bigint|指定OID的数据库使用的磁盘空间|
|pg_database_size(name)|bigint|指定名称的数据库使用的磁盘空间|
|pg_indexes_size(regclass)|bigint|关联指定表OID或表名的表索引的使用总磁盘空间|
|pg_relation_size(relation regclass, fork text)|bigint|指定OID或名的表或索引，通过指定fork('main', 'fsm' 或'vm')所使用的磁盘空间|
|pg_relation_size(relation regclass)|bigint|pg_relation_size(..., 'main')的缩写|
|pg_size_pretty(bigint)|text|Converts a size in bytes expressed as a 64-bit integer into a human-readable format with size units|
|pg_size_pretty(numeric)|text|把以字节计算的数值转换成一个人类易读的尺寸单位|
|pg_table_size(regclass)|bigint|指定表OID或表名的表使用的磁盘空间，除去索引（但是包含TOAST，自由空间映射和可视映射）|
|pg_tablespace_size(oid)|bigint|指定OID的表空间使用的磁盘空间|
|pg_tablespace_size(name)|bigint|指定名称的表空间使用的磁盘空间|
|pg_total_relation_size(regclass)|bigint|指定表OID或表名使用的总磁盘空间，包括所有索引和TOAST数据|

## 例子

- 根据 lsn 获取日志文件名
```shell
template1> select pg_walfile_name('1/4288E228')
+--------------------------+
| pg_walfile_name          |
|--------------------------|
| 000000010000000100000042 |
+--------------------------+
```

- 查看事物 id
```shell
select txid_current()
```

- mvcc 实现自动为表 加 xmin,xmax,cmin,cmax 字段
```shell
create table test(t TEXT, i INT);

postgres@127:postgres> select xmin,xmax,cmin,cmax,* from test;
+------+------+------+------+-------+---+
| xmin | xmax | cmin | cmax | t     | i |
|------+------+------+------+-------+---|
| 740  | 0    | 0    | 0    | 测试2 | 2 |
+------+------+------+------+-------+---+
```

- 查看所有空间占用
```shell
black@127:postgres> select pg_database.datname, pg_database_size(pg_database.datname) AS size from pg_database;
+-----------+---------+
| datname   | size    |
|-----------+---------|
| postgres  | 7680483 |
| template1 | 7746019 |
| template0 | 7520783 |
+-----------+---------+
```
