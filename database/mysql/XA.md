# XA
二阶段提交

## 几种 panic 状态
为了实验充分理解分布式事务

1，在两个db实例prepare之前
    两个事务都没有prepare，全局不可见，异常中断后，本地事务回滚掉了，在两个实例上恢复的时候，都是一致的

2，在db1  prepare之后，db2 prepare之前
    db1，已经prepare了，全局事务会记录下来，本地事务不会自动回滚掉，db2 没有prepare，本地事务会自动回滚掉
    ```shell
    # db1
    xa recover;
    +----------+--------------+--------------+------------+
    | formatID | gtrid_length | bqual_length | data       |
    +----------+--------------+--------------+------------+
    |        1 |           10 |            0 | 1671287200 |
    +----------+--------------+--------------+------------+
    1 row in set (0.00 sec)
    # db2
    xa recover;
    Empty set (0.00 sec)

    ```

    这个时候如果新开一个事务，会等待上一个没有结束的事务释放锁，而超时
    ```shell
    === xid:1671287283 ====
    === call start ====
    Error 1205: Lock wait timeout exceeded; try restarting transaction
    main.main
    ```

3，在两个db prepare之后，db1 commit之前
    两个事务都处于prepare状态，等待处理
    ```shell
    # db1
      mysql> xa recover;
    +----------+--------------+--------------+------------+
    | formatID | gtrid_length | bqual_length | data       |
    +----------+--------------+--------------+------------+
    |        1 |           10 |            0 | 1671287434 |
    +----------+--------------+--------------+------------+
    1 row in set (0.00 sec)
    # db2
      xa recover;
    +----------+--------------+--------------+------------+
    | formatID | gtrid_length | bqual_length | data       |
    +----------+--------------+--------------+------------+
    |        1 |           10 |            0 | 1671287434 |
    +----------+--------------+--------------+------------+
    1 row in set (0.00 sec)
    ```

4，在db1 commit之后， db2 commit之前
    db1已经提交，recover后是空，db2是待提交状态
    ```shell
    # db1
    mysql> xa recover;
    Empty set (0.00 sec)
    # db2
    xa recover;
    +----------+--------------+--------------+------------+
    | formatID | gtrid_length | bqual_length | data       |
    +----------+--------------+--------------+------------+
    |        1 |           10 |            0 | 1671289143 |
    +----------+--------------+--------------+------------+
    1 row in set (0.00 sec)
    ```
    这个时候可以操作db2的回滚，或者提交，db1已经提交，没法回滚了
    ```shell
    # db1
    xa rollback '1671289143';
    ERROR 1397 (XAE04): XAER_NOTA: Unknown XID
    # db2
    xa rollback '1671289143';
    Query OK, 0 rows affected (0.01 sec)
    ```

## 命令
```shell
## 开启本地事务
XA {START|BEGIN} xid [JOIN|RESUME] 

## 结束本地事务
XA END xid 

## 全局事务进入预备状态
XA PREPARE xid  

## 提交 xid
XA COMMIT xid[ONE PHASE] 

## 回滚 xid
XA ROLLBACK xid  

## 查看那些处于 prepare 状态
XA RECOVER[CONVERT XID ]
```

## 确认是否支持
```shell
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

- 单会话测试 xa
```shell
create database test_xa;
use test_xa;
MySQL black@127.0.0.1:test_xa> CREATE TABLE `user` (
                            -> `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
                            -> `score` int(10) unsigned NOT NULL,
                            -> `name` varbinary(255) NOT NULL,
                            -> PRIMARY KEY (`id`)
                            -> );
Query OK, 0 rows affected
Time: 0.060s

MySQL black@127.0.0.1:test_xa> show tables;
+-------------------+
| Tables_in_test_xa |
+-------------------+
| user              |
+-------------------+
1 row in set
Time: 0.008s

MySQL black@127.0.0.1:test_xa> xa start '1000';
Query OK, 0 rows affected
Time: 0.001s

MySQL black@127.0.0.1:test_xa> insert into user values(1, 10, "foo");
Query OK, 1 row affected
Time: 0.002s

MySQL black@127.0.0.1:test_xa> xa recover\G
0 rows in set
Time: 0.001s

MySQL black@127.0.0.1:test_xa> select * from `user`;
+----+-------+------+
| id | score | name |
+----+-------+------+
| 1  | 10    | foo  |
+----+-------+------+
1 row in set
Time: 0.010s

MySQL black@127.0.0.1:test_xa> xa end '1000';
Query OK, 0 rows affected
Time: 0.001s

MySQL black@127.0.0.1:test_xa> xa PREPARE '1000';
Query OK, 0 rows affected
Time: 0.013s


MySQL black@127.0.0.1:test_xa> xa recover;
+----------+--------------+--------------+------+
| formatID | gtrid_length | bqual_length | data |
+----------+--------------+--------------+------+
| 1        | 4            | 0            | 1000 |
+----------+--------------+--------------+------+
1 row in set
Time: 0.011s

MySQL black@127.0.0.1:test_xa> xa commit '1000';
Query OK, 0 rows affected
Time: 0.013s


MySQL black@127.0.0.1:test_xa> xa recover\G
0 rows in set
Time: 0.001s
```
