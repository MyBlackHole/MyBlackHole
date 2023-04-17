# XA
- 方式有
1. memory
2. file (rocksdb)
3. mysql
4. zookeeper

## 命令
```sql
# 提交指定 XID
commit 'dbproxy-0746814c-16';
```

## 是否支持 XA
```sql
show variables like 'innodb_support_xa';
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| innodb_support_xa | ON    |
+-------------------+-------+
1 row in set
Time: 0.014s
```

## 例子

- 单库 XA
```sql
# 数据库\表 需要在 "分布式数据控制台" 创建
CREATE TABLE `article` (
`id` int(10) unsigned NOT NULL AUTO_INCREMENT,
`score` int(10) unsigned NOT NULL,
`name` varbinary(255) NOT NULL,
PRIMARY KEY (`id`)
);

MySQL 5.7.21
mycli 1.24.3
Home: http://mycli.net
Bug tracker: https://github.com/dbcli/mycli/issues
Thanks to the contributor - Michał Górny
MySQL test@192.168.90.207:(none)> show databases;
+----------+
| DATABASE |
+----------+
| UDALTEST |
+----------+
1 row in set
Time: 0.086s
MySQL test@192.168.90.207:(none)> use UDALTEST;
You are now connected to database "UDALTEST" as user "test"
Time: 0.066s
MySQL test@192.168.90.207:UDALTEST> udal xa start;
+--------------------+
| Xid                |
+--------------------+
| dbproxy-0746814c-9 |
+--------------------+
1 row in set
Time: 0.037s
MySQL test@192.168.90.207:UDALTEST> insert into article (score, `name`) values(1000000, "test xa");
Query OK, 1 row affected
Time: 0.090s
MySQL test@192.168.90.207:UDALTEST> udal xa recover;
+------+--------------------+-----------+----------+----------------+
| Id   | Xid                | State     | Run_time | Frontend_state |
+------+--------------------+-----------+----------+----------------+
| 3236 | dbproxy-0746814c-9 | XA_ACTIVE | 28       | alive          |
+------+--------------------+-----------+----------+----------------+
1 row in set
Time: 0.037s
MySQL test@192.168.90.207:UDALTEST> select * from article
+----+---------+----------+
| id | score   | name     |
+----+---------+----------+
| 4  | 10      | foo      |
| 25 | 1000000 | test xa  |
| 15 | 1000    | foo00    |
| 18 | 1000    | foo00000 |
| 21 | 1000    | foo00000 |
| 24 | 1000    | foo00000 |
+----+---------+----------+
6 rows in set
Time: 0.074s
MySQL test@192.168.90.207:UDALTEST> commit;
Query OK, 0 rows affected
Time: 0.026s
MySQL test@192.168.90.207:UDALTEST> udal xa recover;
+----+-----+-------+----------+----------------+
| Id | Xid | State | Run_time | Frontend_state |
+----+-----+-------+----------+----------------+
+----+-----+-------+----------+----------------+
0 rows in set
Time: 0.106s
MySQL test@192.168.90.207:UDALTEST>

```
- 分布式 非 XA 一
```sql
MySQL test@192.168.90.207:UDALTEST> udal dt start;
(3023, 'UDAL - Command execution error: already start xa transaction')
MySQL test@192.168.90.207:UDALTEST> rollback
Query OK, 0 rows affected
Time: 0.003s
MySQL test@192.168.90.207:UDALTEST> udal dt start;
+-------------------------------+
| Transaction_id                |
+-------------------------------+
| teledb04-1681178544061-100000 |
+-------------------------------+
1 row in set
Time: 0.113s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, `name`) values(7, 10000, "hole5");
Query OK, 1 row affected
Time: 0.191s
MySQL test@192.168.90.207:UDALTEST> commit;
Query OK, 0 rows affected
Time: 0.074s
```

- 分布式 XA
```sql
# 数据库\表 需要在 "分布式数据控制台" 创建
CREATE TABLE `article` (
`id` int(10) unsigned NOT NULL AUTO_INCREMENT,
`score` int(10) unsigned NOT NULL,
`name` varbinary(255) NOT NULL,
PRIMARY KEY (`id`)
);

CREATE TABLE `user` (
`id` int(10) unsigned NOT NULL AUTO_INCREMENT,
`score` int(10) unsigned NOT NULL,
`name` varbinary(255) NOT NULL,
PRIMARY KEY (`id`)
);

MySQL test@192.168.90.207:UDALTEST> udal xa start;
Reconnecting...
+---------------------+
| Xid                 |
+---------------------+
| dbproxy-0746814c-13 |
+---------------------+
1 row in set
Time: 0.010s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, `name`) values(19, 10000, "hole5");
Query OK, 1 row affected
Time: 0.007s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, `name`) values(20, 10000, "hole5");
Query OK, 1 row affected
Time: 0.008s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, `name`) values(21, 10000, "hole5");
Query OK, 1 row affected
Time: 0.053s
MySQL test@192.168.90.207:UDALTEST> udal xa recover;
1 row in set
Time: 0.009s
MySQL test@192.168.90.207:UDALTEST> udal xa recover;
+------+---------------------+-----------+----------+----------------+
| Id   | Xid                 | State     | Run_time | Frontend_state |
+------+---------------------+-----------+----------+----------------+
| 3902 | dbproxy-0746814c-13 | XA_ACTIVE | 33       | alive          |
+------+---------------------+-----------+----------+----------------+
1 row in set
Time: 0.050s
MySQL test@192.168.90.207:UDALTEST> select * from user;
+----+-------+-------+
| id | score | name  |
+----+-------+-------+
| 1  | 100   | hole  |
| 7  | 10000 | hole5 |
| 19 | 10000 | hole5 |
| 2  | 10    | hole  |
| 5  | 100   | hole  |
| 8  | 10000 | hole5 |
| 20 | 10000 | hole5 |
| 3  | 100   | hole  |
| 9  | 10000 | hole5 |
| 21 | 10000 | hole5 |
+----+-------+-------+
10 rows in set
Time: 0.033s
MySQL test@192.168.90.207:UDALTEST> commit;
Query OK, 0 rows affected
Time: 0.094s

```
