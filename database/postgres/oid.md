# oid

- 数据库 oid
```shell
select oid,* from pg_database;
```

- 表 oid
```shell
postgres@192:postgres> select oid,relname from pg_class where relname='pg_tablespace';
+------+---------------+
| oid  | relname       |
|------+---------------|
| 1213 | pg_tablespace |
+------+---------------+
SELECT 1
Time: 0.076s
```

- 索引 oid
```shell
select oid,relname from pg_class where relname='pk_account_id';
```

- 对象 oid
```shell
select oid,relname from pg_class where relname='PK_ACCOUNT_ID';
```

- 函数 oid
```shell
select oid,proname from pg_proc where proname='out_tally_pepole_count';
```

- 命名空间 oid
```shell
select oid,nspname from pg_namespace where nspname='comm';
```
