# mysql

## 配置、权限、用户
```shell
show variables like '%data%';
```

- 用户配置
```shell
# 创建用户，授予远程登录权限、操作所有数据权限
create USER BlackHole@'%' identified by '1358244533';
grant all on *.* to BlackHole@'%' with grant option;
flush privileges;
```

- 授权
```shell
格式
grant privilegesCode on dbName.tableName to username@host by "password"; 
(8.0版本，8.0前加上privileges 在on前 ) 

privilegesCode 
    all ：所有权限。 
    select：读取权限。 
    delete：删除权限。 
    update：更新权限。 
    create：创建权限。 
    drop：删除数据库、数据表权限。 
dbName.tableName 
    *.*：授予该数据库服务器所有数据库的权限。 
    dbName.*：授予dbName数据库所有表的权限。 
    dbName.dbTable：授予数据库dbName中dbTable表的权限。 
Host 
    localhost：只允许该用户在本地登录，不能远程登录。 
    %：允许在除本机之外的任何一台机器远程登录。 
    192.168.52.32：具体的IP表示只允许该用户从特定IP登录。 

```

```shell
# 例如
# 8.0前
grant all privileges on minidome.* to Black@'%' identified by '1358244533';  # 授权Blcak具有远程登陆与minidome数据库的所有权限 
# 8.0后
create USER BlackHole@'%' identified by '1358244533';  # 创建具有远程登陆的BlackHole用户 
grant all on minidome.* to BlackHole@'%' with grant option;  # 授予BlackHole具有minidome的所有权限 
flush privileges; # 表示刷新权限变更 
show grants for 'BlackHole'; # 查看权限授予
```

- 创建账号
```shell
create USER BlackHole@'%' identified by '1358244533';  # 创建具有远程登陆的BlackHole用户
```

- 删除
```shell
drop user BlackHole@'localhost';  # 删除用户及权限 
drop database minidome; # 删除数据库
Delete FROM user Where User='Black' and Host='localhost'  # 删除用户
```

## 文件详解

- myisam
1. *.frm--表定义，是描述表结构的文件。
2. *.MYD--"D"数据信息文件，是表的数据文件。
3. *.MYI--"I"索引信息文件，是表数据文件中任何索引的数据树。

- innodb
1. *.frm--表定义，是描述表结构的文件。
2. *.ibd--表数据和索引的文件。该表的索引(B+树)的每个非叶子节点存储索引，叶子节点存储索引和索引对应的数据

## 例子
- 更新
```shell
update mysql.user set password=password('新密码') where User="BlackHole" and Host="localhost"; # 更新密码
```

- 查看表结构
```shell
# 基本结构
describe test2;
# 详细结构 sql
show create table test\G
```

- 查看表索引
```shell
show index from task_instance;
```

## 命令
- 查看正在连接会话
```shell
show processlist;
```

- 查询检测点 
```shell
show engine INNODB STATUS\G
```

- 客户端查看 binlog
```shell
show binlog events in 'binlog.000008';
```

- 查询当前在使用的 bin log
```shell
show master status\G

+---------------+----------+--------------+------------------+-------------------+
| File          | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+---------------+----------+--------------+------------------+-------------------+
| binlog.000009 | 157      |              |                  |                   |
+---------------+----------+--------------+------------------+-------------------+
1 row in set
Time: 0.006s
```

- 查询所有 bin log
```shell
show binary logs;
+---------------+-----------+-----------+
| Log_name      | File_size | Encrypted |
+---------------+-----------+-----------+
| binlog.000001 | 180       | No        |
| binlog.000002 | 404       | No        |
| binlog.000003 | 180       | No        |
| binlog.000004 | 180       | No        |
| binlog.000005 | 180       | No        |
| binlog.000006 | 918       | No        |
| binlog.000007 | 157       | No        |
| binlog.000008 | 2268      | No        |
| binlog.000009 | 157       | No        |
+---------------+-----------+-----------+
9 rows in set
Time: 0.016s
```

- 获取活动的事务
```shell
SELECT * from information_schema.INNODB_TRX;
select concat('KILL ',id,';') from information_schema.processlist p inner join information_schema.INNODB_TRX x on p.id=x.trx_mysql_thread_id where db='user';
```

- 获取活动的锁
```shell
SELECT * FROM information_schema.INNODB_LOCKs;
```

- 锁等待的对应关系
```shell
mysql> SELECT * FROM information_schema.INNODB_LOCK_waits;
```

- 表加读锁
```shell
lock tables user read;
```

- 权限查询
```shell
show grants

# 指定账户
show grants for 'root'@'%'
```

- 获取 redo 大小
```shell
show variables like 'innodb_log_buffer_size';
```

- 获取数据目录路径
```shell
show variables like 'datadir';
```

- 查看用户权限
```shell
show grants for 'root'

select * from mysql.user\G
```

## 连接 mysql
```shell
mysql -h 127.0.0.1 -P 3306 -uroot –p 
```

- 查看表结构
```shell
show columns from mvcctest;
```

- 刷新 binlog, 生成新 binlog 文件
```shell
flush logs;
```

- xtrabackup 备份用户创建，权限
```shell
CREATE USER 'xtrabackup'@'localhost' IDENTIFIED BY 'xtrabackup';
GRANT BACKUP_ADMIN, PROCESS, RELOAD, LOCK TABLES, REPLICATION CLIENT ON *.* TO 'xtrabackup'@'localhost';
GRANT SELECT ON performance_schema.log_status TO 'xtrabackup'@'localhost';
FLUSH PRIVILEGES;

# aio
GRANT BACKUP_ADMIN, RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT, REPLICATION SLAVE, SUPER,BACKUP_ADMIN ON *.* TO 'xtrabackup'@'%';
FLUSH  PRIVILEGES ;
```
