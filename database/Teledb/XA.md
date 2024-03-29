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
## teledb XA 插入分阶段测试
测试结论
teledb XA 执行 commit 时对应 mysql xa end、prepare、commitc;

- binlog 初始状态
```log
SET TIMESTAMP=1681468662/*!*/;
XA START X'646270726f78792d30383734633633322d312d7564616c746573745f322d333133373432',X'',1
/*!*/;
# at 199705143
# at 199705240
#230414 18:37:42 server id 902056666  end_log_pos 199705297 CRC32 0x274005ad    Table_map: `udaltest_2`.`user` mapped to number 111
# at 199705297
#230414 18:37:42 server id 902056666  end_log_pos 199705347 CRC32 0x03ad9726    Write_rows: table id 111 flags: STMT_END_F
### INSERT INTO `udaltest_2`.`user`
### SET
###   @1=981001
###   @2=18
###   @3='test1'
# at 199705347
# at 199705444
#230414 18:37:44 server id 902056666  end_log_pos 199705501 CRC32 0x3bc0e613    Table_map: `udaltest_2`.`user` mapped to number 111
# at 199705501
#230414 18:37:44 server id 902056666  end_log_pos 199705551 CRC32 0x4535b720    Write_rows: table id 111 flags: STMT_END_F
### INSERT INTO `udaltest_2`.`user`
### SET
###   @1=981004
###   @2=50
###   @3='test1'
# at 199705551
#230414 18:40:39 server id 902056666  end_log_pos 199705712 CRC32 0x8c29262b    Query   thread_id=313742        exec_time=0     error_code=0
SET TIMESTAMP=1681468839/*!*/;
XA END X'646270726f78792d30383734633633322d312d7564616c746573745f322d333133373432',X'',1
/*!*/;
# at 199705712
#230414 18:40:39 server id 902056666  end_log_pos 199705784 CRC32 0x11b22356    XA PREPARE X'646270726f78792d30383734633633322d312d7564616c746573745f322d333133373432',X'',1
XA PREPARE X'646270726f78792d30383734633633322d312d7564616c746573745f322d333133373432',X'',1
/*!*/;
# at 199705784
#230414 18:40:39 server id 902056666  end_log_pos 199705849 CRC32 0xa144d15e    GTID    last_committed=379430   sequence_number=379431  rbr_only=no
SET @@SESSION.GTID_NEXT= 'bf8355e5-dbd2-11ec-a00f-005056b1f920:380172'/*!*/;
# at 199705849
#230414 18:40:39 server id 902056666  end_log_pos 199706013 CRC32 0x0f0f5f39    Query   thread_id=313742        exec_time=0     error_code=0
SET TIMESTAMP=1681468839/*!*/;
XA COMMIT X'646270726f78792d30383734633633322d312d7564616c746573745f322d333133373432',X'',1
/*!*/;
ROLLBACK /* added by mysqlbinlog */ /*!*/;
SET @@SESSION.GTID_NEXT= 'AUTOMATIC' /* added by mysqlbinlog */ /*!*/;
DELIMITER ;
# End of log file
```

- 执行 XA start 与 insert 时
binlog 没有变化

- 执行 XA 提交后 有变化 出现了 XA 的各个阶段到日志里

```sql
SET TIMESTAMP=1681694496/*!*/;
XA START X'646270726f78792d30383734633633322d322d7564616c746573745f322d333839383035',X'',1
/*!*/;
# at 199706241
# at 199706338
#230417  9:21:36 server id 902056666  end_log_pos 199706395 CRC32 0x28d31625    Table_map: `udaltest_2`.`user` mapped to number 111
# at 199706395
#230417  9:21:36 server id 902056666  end_log_pos 199706445 CRC32 0x2b70da79    Write_rows: table id 111 flags: STMT_END_F
### INSERT INTO `udaltest_2`.`user`
### SET
###   @1=981007
###   @2=42
###   @3='test1'
# at 199706445
#230417  9:24:00 server id 902056666  end_log_pos 199706606 CRC32 0xe81c996c    Query   thread_id=389805        exec_time=0     error_code=0
SET TIMESTAMP=1681694640/*!*/;
XA END X'646270726f78792d30383734633633322d322d7564616c746573745f322d333839383035',X'',1
/*!*/;
# at 199706606
#230417  9:24:00 server id 902056666  end_log_pos 199706678 CRC32 0x852853c2    XA PREPARE X'646270726f78792d30383734633633322d322d7564616c746573745f322d333839383035',X'',1
XA PREPARE X'646270726f78792d30383734633633322d322d7564616c746573745f322d333839383035',X'',1
/*!*/;
# at 199706678
#230417  9:24:00 server id 902056666  end_log_pos 199706743 CRC32 0x5369b2a8    GTID    last_committed=379432   sequence_number=379433  rbr_only=no
SET @@SESSION.GTID_NEXT= 'bf8355e5-dbd2-11ec-a00f-005056b1f920:380174'/*!*/;
# at 199706743
#230417  9:24:00 server id 902056666  end_log_pos 199706907 CRC32 0xd95a2dbc    Query   thread_id=389805        exec_time=0     error_code=0
SET TIMESTAMP=1681694640/*!*/;
XA COMMIT X'646270726f78792d30383734633633322d322d7564616c746573745f322d333839383035',X'',1
/*!*/;
ROLLBACK /* added by mysqlbinlog */ /*!*/;
```
