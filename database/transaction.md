# 事务

atomicity(原子性)
- 事务是不可再分割的工作单位,事务中的操作要么都发生,要么都不发生

consistency(一致性)
- 在事务开始之前和事务结束以后,数据库的完整性约束没有被破坏
  这是说数据库事务不能破坏关系数据的完整性以及业务逻辑上的一致性

lsolation(隔离性)
- 多个事务并发访问时,事务之间是隔离的,一个事务不应该影响其它事务运行效果
  这指的是在并发环境中,当不同的事务同时操纵相同的数据时,每个事务都有各自的完整数据空间
  由并发事务所做的修改必须与任何其他并发事务所做的修改隔离
  事务查看数据更新时,数据所处的状态要么是另一事务修改它之前的状态,要么是另一事务修改它之后的状态,事务不会查看到中间状态的数据
  这是说数据库事务不能破坏关系数据的完整性以及业务逻辑上的一致性

durability(持久性)
- 持久性,意味着在事务完成以后,该事务所对数据库所作的更改便持久的保存在数据库之中,并不会被回滚

设置方式
```bash
# 在my.inf文件的[mysqld]节里类似如下设置该选项
transaction-isolation = {READ-UNCOMMITTED | READ-COMMITTED | REPEATABLE-READ | SERIALIZABLE}
```
或
```sql
SET [SESSION | GLOBAL] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE}
```
注意：默认的行为(不带session和global)是为下一个(未开始)事务设置隔离级别
      如果你使用GLOBAL关键字,语句在全局对从那点开始创建的所有新连接(除了不存在的连接)设置默认事务级别
      你需要SUPER权限来做这个
      使用SESSION 关键字为将来在当前连接上执行的事务设置默认事务级别
      任何客户端都能自由改变会话隔离级别(甚至在事务的中间),或者为下一个事务设置隔离级别

查询方式
```
# 版本小于 8.x
SELECT @@global.tx_isolation;
SELECT @@session.tx_isolation;
SELECT @@tx_isolation;

# 版本大于等于 8.x
SELECT @@global.transaction_isolation;
SELECT @@session.transaction_isolation;
SELECT @@transaction_isolation;

```

## 事务隔离
![[imgs/GetImage 1.jpeg]]
![[imgs/GetImage 2.jpeg]]
innoDB默认是可重复读

未提交读(read uncommitted)
- 允许脏读,也就是可能读取到其他会话中未提交事务修改的数据

提交读(read committed)
- 只能读取到已经提交的数据。Oracle等多数数据库默认都是该级别(不可重复读)

可重复读(repeated read)
- 在同一个事务内的查询都是事务开始时刻一致的,InnoDB默认级别
  在SQL标准中,该隔离级别消除了不可重复读,但是还存在幻象读 

串行读(serializable)
- 完全串行化的读,每次读都需要获得表级共享锁,读写相互都会阻塞 

## 级别情况
脏读
- 就是指当一个事务正在访问数据,并且对数据进行了修改,而这种修改还没有提交到数据库中,这时,另外一个事务也访问这个数据,然后使用了这个数据
```sql
# 会话1
MySQL root@127.0.0.1:test> SELECT @@global.transaction_isolation;
+--------------------------------+
| @@global.transaction_isolation |
+--------------------------------+
| REPEATABLE-READ                |
+--------------------------------+

MySQL root@127.0.0.1:test> SELECT @@session.transaction_isolation;
+---------------------------------+
| @@session.transaction_isolation |
+---------------------------------+
| REPEATABLE-READ                 |
+---------------------------------+


MySQL root@127.0.0.1:test> start transaction;
Query OK, 0 rows affected
Time: 0.001s


MySQL root@127.0.0.1:test> insert into t values(1);
Query OK, 1 row affected
Time: 0.001s


MySQL root@127.0.0.1:test> select * from t;
+---+
| i |
+---+
| 1 |
+---+


# 会话2
MySQL root@127.0.0.1:test> select @@session.transaction_isolation;
+---------------------------------+
| @@session.transaction_isolation |
+---------------------------------+
| REPEATABLE-READ                 |
+---------------------------------+


MySQL root@127.0.0.1:test> select * from t;    # 不会读取到会话1未提交的插入
+---+
| i |
+---+
+---+


MySQL root@127.0.0.1:test> set session transaction isolation level read uncommitted; # 修改事务级别为 “未提交读”
Query OK, 0 rows affected
Time: 0.001s


MySQL root@127.0.0.1:test> select * from t;   # 读取到了会话1未提交的数据
+---+
| i |
+---+
| 1 |
+---+
```

### 不可重复读
- 是指在一个事务内,多次读同一数据
  在这个事务还没有结束时,另外一个事务也访问该同一数据
  那么,在第一个事务中的两次读数据之间,由于第二个事务的修改,那么第一个事务两次读到的的数据可能是不一样的
  这样就发生了在一个事务内两次读到的数据是不一样的,因此称为是不可重复读
```sql
# 会话1
MySQL root@127.0.0.1:test> select @@session.transaction_isolation;
+---------------------------------+
| @@session.transaction_isolation |
+---------------------------------+
| READ-COMMITTED                  |
+---------------------------------+


MySQL root@127.0.0.1:test> start transaction;
Query OK, 0 rows affected
Time: 0.000s


MySQL root@127.0.0.1:test> select * from t;
+---+
| i |
+---+
| 1 |
+---+

# 会话2
MySQL root@127.0.0.1:test> select @@session.transaction_isolation;
+---------------------------------+
| @@session.transaction_isolation |
+---------------------------------+
| REPEATABLE-READ                 |
+---------------------------------+


MySQL root@127.0.0.1:test> select * from t;
+---+
| i |
+---+
| 1 |
+---+


MySQL root@127.0.0.1:test> start transaction;
Query OK, 0 rows affected
Time: 0.000s


MySQL root@127.0.0.1:test> insert into t values(2);
Query OK, 1 row affected
Time: 0.014s


MySQL root@127.0.0.1:test> select * from t;
+---+
| i |
+---+
| 1 |
| 2 |
+---+


MySQL root@127.0.0.1:test> commit;
Query OK, 0 rows affected
Time: 0.001s


# 会话1
MySQL root@127.0.0.1:test> select * from t; # 还在事务内前后读取到不同内容
+---+
| i |
+---+
| 1 |
| 2 |
+---+
```

- 可重复读
```sql
# 会话1
MySQL root@127.0.0.1:test> select @@session.transaction_isolation;
+---------------------------------+
| @@session.transaction_isolation |
+---------------------------------+
| REPEATABLE-READ                 |
+---------------------------------+


MySQL root@127.0.0.1:test> start transaction;
Query OK, 0 rows affected
Time: 0.002s


MySQL root@127.0.0.1:test> select * from t;
+---+
| i |
+---+
| 1 |
| 2 |
+---+


# 会话2
MySQL root@127.0.0.1:test> select @@session.transaction_isolation;
+---------------------------------+
| @@session.transaction_isolation |
+---------------------------------+
| REPEATABLE-READ                 |
+---------------------------------+


MySQL root@127.0.0.1:test> start transaction;
Query OK, 0 rows affected
Time: 0.001s


MySQL root@127.0.0.1:test> insert into t values(3);
Query OK, 1 row affected
Time: 0.002s


MySQL root@127.0.0.1:test> commit;
Query OK, 0 rows affected
Time: 0.013s


MySQL root@127.0.0.1:test> select * from t;
+---+
| i |
+---+
| 1 |
| 2 |
| 3 |
+---+


# 会话1
MySQL root@127.0.0.1:test> select * from t; # 事务过程中读取前后数据一致
+---+
| i |
+---+
| 1 |
| 2 |
+---+
```


幻读
- 第一个事务对一个表中的数据进行了修改,这种修改涉及到表中的全部数据行
  同时,第二个事务也修改这个表中的数据,这种修改是向表中插入一行新数据
  那么,以后就会发生操作第一个事务的用户发现表中还有没有修改的数据行,就好象发生了幻觉一样
```sql
mysql>CREATE TABLE `t_bitfly` (
`id` bigint(20) NOT NULL default '0',
`value` varchar(32) default NULL,
PRIMARY KEY (`id`)
) ENGINE=InnoDB

mysql> select @@global.tx_isolation, @@tx_isolation;
+-----------------------+-----------------+
| @@global.tx_isolation | @@tx_isolation  |
+-----------------------+-----------------+
| REPEATABLE-READ       | REPEATABLE-READ |
+-----------------------+-----------------+

实验一：

t Session A                   Session B
|
| START TRANSACTION;          START TRANSACTION;
|
| SELECT * FROM t_bitfly;
| empty set
|                             INSERT INTO t_bitfly
|                             VALUES (1, 'a');
|
| SELECT * FROM t_bitfly;
| empty set
|                             COMMIT;
|
| SELECT * FROM t_bitfly;
| empty set
|
| INSERT INTO t_bitfly VALUES (1, 'a');
| ERROR 1062 (23000):
| Duplicate entry '1' for key 1
v (shit, 刚刚明明告诉我没有这条记录的)

如此就出现了幻读，以为表里没有数据，其实数据已经存在了，傻乎乎的提交后，才发现数据冲突了。

实验二：

t Session A                  Session B
|
| START TRANSACTION;         START TRANSACTION;
|
| SELECT * FROM t_bitfly;
| +------+-------+
| | id   | value |
| +------+-------+
| |    1 | a     |
| +------+-------+
|                            INSERT INTO t_bitfly
|                            VALUES (2, 'b');
|
| SELECT * FROM t_bitfly;
| +------+-------+
| | id   | value |
| +------+-------+
| |    1 | a     |
| +------+-------+
|                            COMMIT;
|
| SELECT * FROM t_bitfly;
| +------+-------+
| | id   | value |
| +------+-------+
| |    1 | a     |
| +------+-------+
|
| UPDATE t_bitfly SET value='z';
| Rows matched: 2  Changed: 2  Warnings: 0
| (怎么多出来一行)
|
| SELECT * FROM t_bitfly;
| +------+-------+
| | id   | value |
| +------+-------+
| |    1 | z     |
| |    2 | z     |
| +------+-------+

实验三：t Session A                 Session B
|
| START TRANSACTION;        START TRANSACTION;
|
| SELECT * FROM t_bitfly
| WHERE id<=1
| FOR UPDATE;
| +------+-------+
| | id   | value |
| +------+-------+
| |    1 | a     |
| +------+-------+
|                           INSERT INTO t_bitfly
|                           VALUES (2, 'b');
|                           Query OK, 1 row affected
|
| SELECT * FROM t_bitfly;
| +------+-------+
| | id   | value |
| +------+-------+
| |    1 | a     |
| +------+-------+
|                           INSERT INTO t_bitfly
|                           VALUES (0, '0');
|                           (waiting for lock ...then timeout)
|                           ERROR 1205 (HY000):
|                           Lock wait timeout exceeded;
|                           try restarting transaction
|
| SELECT * FROM t_bitfly;
| +------+-------+
| | id   | value |
| +------+-------+
| |    1 | a     |
| +------+-------+
|                           COMMIT;
|
| SELECT * FROM t_bitfly;

| +------+-------+
| | id   | value |
| +------+-------+
| |    1 | a     |
| +------+-------+

实验四：一致性读和提交读t Session A                      Session B
|
| START TRANSACTION;             START TRANSACTION;
|
| SELECT * FROM t_bitfly;
| +----+-------+
| | id | value |
| +----+-------+
| |  1 | a     |
| +----+-------+
|                                INSERT INTO t_bitfly
|                                VALUES (2, 'b');
|                                COMMIT;
|
| SELECT * FROM t_bitfly;
| +----+-------+
| | id | value |
| +----+-------+
| |  1 | a     |
| +----+-------+
|
| SELECT * FROM t_bitfly LOCK IN SHARE MODE;
| +----+-------+
| | id | value |
| +----+-------+
| |  1 | a     |
| |  2 | b     |
| +----+-------+
|
| SELECT * FROM t_bitfly FOR UPDATE;
| +----+-------+
| | id | value |
| +----+-------+
| |  1 | a     |
| |  2 | b     |
| +----+-------+
|
| SELECT * FROM t_bitfly;
| +----+-------+
| | id | value |
| +----+-------+
| |  1 | a     |
| +----+-------+
```
## 总结
- 理解事务的4个特性：原子性、一致性、隔离性、持久性 
- 掌握事务操作常见命令的介绍 
- set autocommit可以设置是否开启自动提交事务 
- start transaction：开启事务 
- start transaction read only：开启只读事物 
- commit：提交事务 
- rollback：回滚事务 
- savepoint：设置保存点 
- rollback to 保存点：可以回滚到某个保存点 
- 掌握4种隔离级别及了解其特点 
- 脏读、不可重复读、幻读 



> [MySQL 四种事务隔离级的说明](https://www.cnblogs.com/zhoujinyi/p/3437475.HTML)
