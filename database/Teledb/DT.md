# DT
TeleDB DT 指的是补偿型

- 单库事务 非 XA
```sql
MySQL test@192.168.90.207:UDALTEST> dt start user;
+-----------------------------+
| TransactionId               |
+-----------------------------+
| teledb04-1681178925301-user |
+-----------------------------+
1 row in set
Time: 0.069s
MySQL test@192.168.90.207:UDALTEST> show warnings
+-------+------+---------+
| Level | Code | Message |
+-------+------+---------+
+-------+------+---------+
0 rows in set
Time: 0.100s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, `name`) values(8, 10000, "hole5");
Query OK, 1 row affected
Time: 0.036s
MySQL test@192.168.90.207:UDALTEST> show warnings
+-------+------+---------+
| Level | Code | Message |
+-------+------+---------+
+-------+------+---------+
0 rows in set
Time: 0.085s
MySQL test@192.168.90.207:UDALTEST> commit;
```

- 分布式 非 XA 二
```sql
MySQL test@192.168.90.207:UDALTEST> dt start user;
+-----------------------------+
| TransactionId               |
+-----------------------------+
| teledb04-1681178925301-user |
+-----------------------------+
1 row in set
Time: 0.069s
MySQL test@192.168.90.207:UDALTEST> show warnings
+-------+------+---------+
| Level | Code | Message |
+-------+------+---------+
+-------+------+---------+
0 rows in set
Time: 0.100s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, `name`) values(8, 10000, "hole5");
Query OK, 1 row affected
Time: 0.036s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, `name`) values(9, 10000, "hole5");
Query OK, 1 row affected
Time: 0.097s
MySQL test@192.168.90.207:UDALTEST> show warnings
+-------+------+---------+
| Level | Code | Message |
+-------+------+---------+
+-------+------+---------+
0 rows in set
Time: 0.085s
MySQL test@192.168.90.207:UDALTEST> commit;
```
