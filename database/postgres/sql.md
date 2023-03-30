# sql


## 例子

- 根据 lsn 获取日志文件名
```sql
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
