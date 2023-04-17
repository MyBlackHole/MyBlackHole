# DT 事务测试

- 测试简单插入
binlog 起始状态
```sql
DELIMITER /*!*/;
# at 4
#230417 11:02:53 server id 902056666  end_log_pos 123 CRC32 0x0ec2ea8a  Start: binlog v 4, server v 5.7.36-log created 230417 11:02:53 at startup
# Warning: this binlog is either in use or was not closed properly.
ROLLBACK/*!*/;
# at 123
#230417 11:02:53 server id 902056666  end_log_pos 274 CRC32 0x0ce3e35a  Previous-GTIDs
# bde4de15-dbd2-11ec-9878-005056b12728:1-14,
# bf8355e5-dbd2-11ec-a00f-005056b1f920:1-380183,
# bf987d8a-dbd2-11ec-aeb1-005056b1c227:1-332
# at 274
#230417 11:06:00 server id 902056666  end_log_pos 339 CRC32 0xdddaeef5  GTID    last_committed=0        sequence_number=1       rbr_only=yes
/*!50718 SET TRANSACTION ISOLATION LEVEL READ COMMITTED*//*!*/;
SET @@SESSION.GTID_NEXT= 'bf8355e5-dbd2-11ec-a00f-005056b1f920:380184'/*!*/;
# at 339
```

- 执行 UDAL DT START binlo 没有变化
- 执行 insert into user (score, name) values( RAND() * 100, 'test1'); 没有变化
- 执行 commit
```sql
BEGIN
/*!*/;
# at 417
# at 514
#230417 11:04:36 server id 902056666  end_log_pos 571 CRC32 0x1bdc6dd8  Table_map: `udaltest_2`.`user` mapped to number 108
# at 571
#230417 11:04:36 server id 902056666  end_log_pos 621 CRC32 0x97850d23  Write_rows: table id 108 flags: STMT_END_F
### INSERT INTO `udaltest_2`.`user`
### SET
###   @1=981034
###   @2=87
###   @3='test1'
# at 621
#230417 11:06:00 server id 902056666  end_log_pos 652 CRC32 0x4dce7c0b  Xid = 123
COMMIT/*!*/;
```

## 断开某个节点
打开防火墙拦截所有 外来访问 6666 端口的连接
kill mysql_safe 6666 的 pid
所有节点 的 节点事务都会没有提交

## 增加网络延迟测试
tc qdisc change dev ens192 root netem delay 150000ms
- 初始状态
```sql
MySQL test@192.168.90.207:UDALTEST> select * from `user`;
+----+-------+-------+
| id | score | name  |
+----+-------+-------+
| 6  | 15    | test1 |
| 9  | 10000 | hole5 |
| 2  | 10    | hole  |
| 5  | 100   | hole  |
| 8  | 10000 | hole5 |
| 1  | 100   | hole  |
| 7  | 10000 | hole5 |
| 10 | 95    | test1 |
+----+-------+-------+
```

- 执行 sql
```sql
MySQL test@192.168.90.207:UDALTEST> UDAL DT START;
+-------------------------------+
| Transaction_id                |
+-------------------------------+
| teledb04-1681711132709-100149 |
+-------------------------------+
1 row in set
Time: 0.073s
MySQL test@192.168.90.207:UDALTEST> insert into user (score, name) values( RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.060s
MySQL test@192.168.90.207:UDALTEST> insert into user (score, name) values( RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.081s
MySQL test@192.168.90.207:UDALTEST> insert into user (score, name) values( RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.178s
MySQL test@192.168.90.207:UDALTEST> select * from `user`;
+---------+-------+-------+
| id      | score | name  |
+---------+-------+-------+
| 6       | 15    | test1 |
| 9       | 10000 | hole5 |
| 1103451 | 50    | test1 |
| 2       | 10    | hole  |
| 5       | 100   | hole  |
| 8       | 10000 | hole5 |
| 1103450 | 92    | test1 |
| 1       | 100   | hole  |
| 7       | 10000 | hole5 |
| 10      | 95    | test1 |
| 1103452 | 89    | test1 |
+---------+-------+-------+
11 rows in set
Time: 0.018s
MySQL test@192.168.90.207:UDALTEST> commit;
Query OK, 0 rows affected
Time: 150.042s
```

- 执行 sql 后
```shell
MySQL test@192.168.90.207:UDALTEST> select * from `user`;
+---------+-------+-------+
| id      | score | name  |
+---------+-------+-------+
| 6       | 15    | test1 |
| 9       | 10000 | hole5 |
| 1103451 | 50    | test1 |
| 2       | 10    | hole  |
| 5       | 100   | hole  |
| 8       | 10000 | hole5 |
| 1103450 | 92    | test1 |
| 1       | 100   | hole  |
| 7       | 10000 | hole5 |
| 10      | 95    | test1 |
| 1103452 | 89    | test1 |
+---------+-------+-------+
11 rows in set
Time: 0.134s
```

## 设置丢包率
