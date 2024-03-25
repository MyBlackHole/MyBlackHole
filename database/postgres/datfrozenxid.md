# datfrozenxid

在PostgreSQL中，每个事务都有一个唯一的事务ID(Transaction ID), 也称为XID。
当一个事务被提交时，它的XID将被写入数据库的相关目录中。
datfrozenxid 是一个系统视图 pg_database 的列，用于记录每个数据库中最旧的未清理的事务ID

用作是处理 txid 循环问题

## age(datfrozenxid) 的含义
age(datfrozenxid) 是一个用于计算数据库的最旧未清理事务ID的时间跨度的函数。
它返回一个间隔类型的值，表示最旧的未清理的事务ID的创建时间到当前时间之间的时间跨度

## 如何计算 age(datfrozenxid)
```shell
SELECT age(datfrozenxid) AS frozen_age
FROM pg_database
WHERE datname = 'your_database_name';
```

```shell
SELECT age(datfrozenxid) AS frozen_age
FROM pg_database
WHERE datname = 'example_db';

这将返回一个时间跨度值，表示最旧未清理的事务ID的创建时间到当前时间之间的时间跨度。
```
