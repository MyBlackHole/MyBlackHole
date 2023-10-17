# mvcc

## 四种现象

1. 脏读 dirty read（读到未提交的数据）
A事务正在修改数据但未提交，此时B事务去读取此条数据，B事务读取的是未提交的数据，A事务回滚
解决办法：
方法1：事务隔离级别设置为：read committed。
方法2：读取时加排它锁（select…for update），事务提交才会释放锁，修改时加共享锁（update …lock in share mode)。加排它锁后，不能对该条数据再加锁，能查询但不能更改数据。mysql InnoDB引擎默认的修改数据语句，update,delete,insert都会自动给涉及到的数据加上排他锁，select语句默认不会加任何锁类型，如果加排他锁可以使用select …for update语句，如果事务T对数据A加上共享锁后，则其他事务只能对A再加共享锁，不能加排他锁，共享锁下其它用户可以并发读取，查询数据。但不能修改，增加，删除数据。资源共享

2. 不可重复读 Non-Repeatable read（前后多次读取，数据内容不一致）
A事务中两次查询同一数据的内容不同，B事务间在A事务两次读取之间更改了此条数据。
解决办法：
方法1：事务隔离级别设置为Repeatable read。
方法2：读取数据时加共享锁，写数据时加排他锁，都是事务提交才释放锁
    读取时候不允许其他事物修改该数据，不管数据在事务过程中读取多少次，数据都是一致的，避免了不可重复读问题。

3. 幻读 repeatable read（前后多次读取，数据总量不一致）
在同一事务中两次相同查询数据的条数不一致，例如第一次查询查到5条数据，第二次查到8条数据，这是因为在两次查询的间隙，另一个事务插入了3条数据。
解决办法：
方法1：事务隔离级别设置为serializable ，那么数据库就变成了单线程访问的数据库，导致性能降低很多。

DEFAULT： 使用数据库设置的隔离级别 ( 默认 ) ，由 DBA 默认的设置来决定隔离级别 .

READ_UNCOMMITTED： 会读到未提交的数据， 出现脏读、不可重复读、幻读 ( 隔离级别最低，并发性能高 )。

READ_COMMITTED： 不会读到未提交的数据，会出现不可重复读、幻读问题（锁定正在读取的行）

REPEATABLE_READ ：会出幻读（锁定所读取的所有行）

SERIALIZABLE ：保证所有的情况不会发生（锁表）。


## 四种不同的隔离级别含义分别如下

- 序列化（SERIALIZABLE）
SERIALIZABLE:
  如果隔离级别为序列化，则用户之间通过一个接一个顺序地执行当前的事务，这种隔离级别提供了事务之间最大限度的隔离。

- 可重复读（REPEATABLE READ）
REPEATABLE READ:
  在可重复读在这一隔离级别上，事务不会被看成是一个序列。
  不过，当前正在执行事务的变化仍然不能被外部看到，也就是说，如果用户在另外一个事务中执行同条 SELECT 语句数次，结果总是相同的。
  （因为正在执行的事务所产生的数据变化不能被外部看到）。

- 提交读（READ COMMITTED）
READ COMMITTED:
  READ COMMITTED 隔离级别的安全性比 REPEATABLE READ 隔离级别的安全性要差。
  处于 READ COMMITTED 级别的事务可以看到其他事务对数据的修改。也就是说，在事务处理期间，
  如果其他事务修改了相应的表，那么同一个事务的多个 SELECT 语句可能返回不同的结果。

- 未提交读（READ UNCOMMITTED）
READ UNCOMMITTED:
  READ UNCOMMITTED 提供了事务之间最小限度的隔离。
  除了容易产生虚幻的读操作和不能重复的读操作外，处于这个隔离级的事务可以读到其他事务还没有提交的数据，
  如果这个事务使用其他事务不提交的变化作为计算的基础，然后那些未提交的变化被它们的父事务撤销，这就导致了大量的数据变化。

## 测试

1. 预备阶段

MySQL8 之前使用如下命令查看 MySQL 隔离级别:
```shell
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
```

MySQL8 开始，通过如下命令查看 MySQL 默认隔离级别：
```shell
select @@GLOBAL.transaction_isolation, @@transaction_isolation;

+--------------------------------+-------------------------+
| @@GLOBAL.transaction_isolation | @@transaction_isolation |
+--------------------------------+-------------------------+
| REPEATABLE-READ                | REPEATABLE-READ         |
+--------------------------------+-------------------------+
1 row in set (0.00 sec)
```


```shell
# 通过如下命令可以修改隔离级别（建议开发者在修改时修改当前 session 隔离级别即可，不用修改全局的隔离级别）：
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;


  # 查看所有数据库
  show databases;

  # 创建数据库
  create database test charset=utf8;

  # 切换数据库
  use test;

  # 查看当前使用数据库
  select database()

  # 创建表
  create table mvcctest (
    id int not null auto_increment,
    name varchar(20) not null,
    balance int not null,
    PRIMARY KEY(id)
  );

  # 查看当前使用数据库所有表
  show tables;

  # 查看表结构
  show columns from mvcctest;

  +---------+-------------+------+-----+---------+----------------+
  | Field   | Type        | Null | Key | Default | Extra          |
  +---------+-------------+------+-----+---------+----------------+
  | id      | int         | NO   | PRI | NULL    | auto_increment |
  | name    | varchar(20) | NO   |     | NULL    |                |
  | balance | int         | NO   |     | NULL    |                |
  +---------+-------------+------+-----+---------+----------------+
  3 rows in set (0.01 sec)


  # 插入数据
  insert into mvcctest(name, balance) values("javaboy", 1000), ("itboyhub", 1000);
```

2. 未提交读（READ UNCOMMITTED）
  1. 脏读
  一个事务读到另外一个事务还没有提交的数据，称之为脏读。具体操作如下：
  A、B 为两个窗口,都是未提交读会话：

  初始数据:
    +----+----------+---------+
    | id | name     | balance |
    +----+----------+---------+
    |  1 | javaboy  |    1000 |
    |  2 | itboyhub |    1000 |
    +----+----------+---------+

    B:
      # 开启事务
      START TRANSACTION;
    A:
      先执行:
      START TRANSACTION;
      UPDATE mvcctest set balance=balance+100 where name='javaboy';
    B:
      SELECT * from mvcctest;

      +----+----------+---------+
      | id | name     | balance |
      +----+----------+---------+
      |  1 | javaboy  |    1100 |
      |  2 | itboyhub |    1000 |
      +----+----------+---------+
      2 rows in set (0.00 sec)

      # 提交事务
      COMMIT;
    A:
      UPDATE mvcctest set balance=balance-100 where name='itboyhub';
      COMMIT;

  2. 不可重复读
  不可重复读是指一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读。
  具体操作步骤如下（操作之前先将两个账户的钱都恢复为1000）：

  +----+----------+---------+
  | id | name     | balance |
  +----+----------+---------+
  |  1 | javaboy  |    1000 |
  |  2 | itboyhub |    1000 |
  +----+----------+---------+
  2 rows in set (0.00 sec)


    B:
      START TRANSACTION;
      SELECT * from mvcctest where name='javaboy';

      +----+---------+---------+
      | id | name    | balance |
      +----+---------+---------+
      |  1 | javaboy |    1000 |
      +----+---------+---------+
      1 row in set (0.00 sec)
    A:
      START TRANSACTION;
      UPDATE mvcctest set balance=balance+100 where name='javaboy';
      COMMIT;
    B:
      SELECT * from mvcctest where name='javaboy';

      +----+---------+---------+
      | id | name    | balance |
      +----+---------+---------+
      |  1 | javaboy |    1100 |
      +----+---------+---------+
      1 row in set (0.00 sec)

      COMMIT;

  3. 幻读
  幻象读和不可重复读非常像，看名字就是产生幻觉了。

    B:
      START TRANSACTION;
      SELECT * from mvcctest;

      +----+----------+---------+
      | id | name     | balance |
      +----+----------+---------+
      |  1 | javaboy  |    1100 |
      |  2 | itboyhub |    1000 |
      +----+----------+---------+
      2 rows in set (0.00 sec)
    A:
      START TRANSACTION;
      insert into mvcctest(name,balance) values('zhangsan',1000);
    B:
      SELECT * from mvcctest;

      +----+----------+---------+
      | id | name     | balance |
      +----+----------+---------+
      |  1 | javaboy  |    1100 |
      |  2 | itboyhub |    1000 |
      |  3 | zhangsan |    1000 |
      +----+----------+---------+
      3 rows in set (0.00 sec)
    B:
      delete from mvcctest where name='zhangsan';

      # 堵塞
    A:
      COMMIT;
      # 提交后 B 删除成功
    B:
      COMMIT;


3. 提交读（READ COMMITTED）
  和 READ UNCOMMITTED 相比，READ COMMITTED 主要解决了脏读的问题，对于**不可重复读**和**幻象读**则未解决。

    A:
      SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

      select @@GLOBAL.transaction_isolation, @@transaction_isolation;

      +--------------------------------+-------------------------+
      | @@GLOBAL.transaction_isolation | @@transaction_isolation |
      +--------------------------------+-------------------------+
      | REPEATABLE-READ                | READ-COMMITTED          |
      +--------------------------------+-------------------------+
      1 row in set (0.00 sec)

    B:
      SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

      select @@GLOBAL.transaction_isolation, @@transaction_isolation;

      +--------------------------------+-------------------------+
      | @@GLOBAL.transaction_isolation | @@transaction_isolation |
      +--------------------------------+-------------------------+
      | REPEATABLE-READ                | READ-COMMITTED          |
      +--------------------------------+-------------------------+
      1 row in set (0.00 sec)


    B:
      START TRANSACTION;
      SELECT * from mvcctest;

      +----+----------+---------+
      | id | name     | balance |
      +----+----------+---------+
      |  1 | javaboy  |    1100 |
      |  2 | itboyhub |    1000 |
      +----+----------+---------+
      2 rows in set (0.00 sec)

    A:
      START TRANSACTION;
      insert into mvcctest(id,name,balance) values(11, 'zhangsan',1000);

    B:
      SELECT * from mvcctest;

      +----+----------+---------+
      | id | name     | balance |
      +----+----------+---------+
      |  1 | javaboy  |    1100 |
      |  2 | itboyhub |    1000 |
      +----+----------+---------+
      2 rows in set (0.00 sec)

    B:
      insert into mvcctest(id,name,balance) values(11, 'zhangsan',1000);

      # 堵塞
      # 报错

    A:
      COMMIT;

    B:
      COMMIT;
