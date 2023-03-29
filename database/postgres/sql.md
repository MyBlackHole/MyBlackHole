# sql


## 例子

- 二阶段提交 XA
```shell
black@127:black> begin;
BEGIN
Time: 0.001s
black@127:black> create table demo(a TEXT, b INTEGER);
CREATE TABLE
Time: 0.013s
black@127:black> PREPARE TRANSACTION 'the first prepared transaction';
PREPARE TRANSACTION
Time: 0.008s
black@127:black> select * FROM pg_prepared_xacts;
+-------------+--------------------------------+------------------------------+-------+----------+
| transaction | gid                            | prepared                     | owner | database |
|-------------+--------------------------------+------------------------------+-------+----------|
| 738         | the first prepared transaction | 2023-03-29 22:18:01.90887+08 | black | black    |
+-------------+--------------------------------+------------------------------+-------+----------+
SELECT 1
Time: 0.006s
black@127:black> ROLLBACK PREPARED 'the first prepared transaction';
ROLLBACK PREPARED
Time: 0.009s
black@127:black> select * FROM pg_prepared_xacts;
+-------------+-----+----------+-------+----------+
| transaction | gid | prepared | owner | database |
|-------------+-----+----------+-------+----------|
+-------------+-----+----------+-------+----------+
SELECT 0
Time: 0.009s
black@127:black>
```
