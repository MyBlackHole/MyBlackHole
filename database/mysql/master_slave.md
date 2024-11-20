# 主从
1. 做数据的热备，将从库作为后备数据库，主数据库服务器故障后，可切换到从数据库继续工作，避免数据丢失。
2. 满足业务需求，当业务量越来越大，O/I频率越来越高，单个主机已经无法满足需求，增添架构，可以分担业务量，提升O/I性能，也就是所谓的负载均衡。
3. 为MySQL其他架构做基础，例如：读写分离，高可用。


部署从库创建SQL线程，读取relay log日志中的内容，并解析执行，完成数据同步。
MySQL主从复制就是将数据库群中一台或多台服务器作为主（master）数据库，其他数据库作为从（slave）数据库，然后指向主库，实时同步主库中的数据；当主库数据发生变化时，会将变化的数据实时同步到一个或多个从库

## 过程
![[imgs/master_slave.png]]

- Binary log dump 线程
当从节点连接主节点后，主节点会创建一个log dump线程，用于发送binlog日志中的内容

- I/O线程
当从节点执行"start slave"命令后，从节点会创建一个I/O线程用来连接主节点，请求主库更新的binlog日志。I/O线程接收到主节点log dump线程发来的更新数据之后，保存到本地的relay log日志

- SQL线程
SQL线程读取relay log日志中的内容，然后解析成具体的SQL语句并执行，最终完成数据同步

1. 主节点数据更新，随后将具体的更新语句写到binlog日志中
2. 从节点开启主从复制功能，创建一个I/O线程，连接主节点
3. 主节点接收到从节点的连接后，创建一个binary log dump线程，把binlog日志中更新的内容发送到从节点
4. 从节点的I/O线程读取主节点发送的binlog日志，并写入到relay log中
5. 从库创建SQL线程，读取relay log日志中的内容，并解析执行，完成数据同步


## 二进制日志格式

基于SQL语句复制（statement-based replication，简称SBR）
基于行的复制（row-based replication，简称RBR）
混合模式复制（mixed-based replication，简称MBR）

### STATEMENT 模式（SBR）
会将数据修改的SQL语句写到binlog日志中，无需记录每行数据变化，减少了日志量，但有些数据无法保存，不能保证数据的一致性。

### ROW 模式(RBR)
此模式为默认。
仅记录数据被修改，修改成什么样子，不记录SQL语句信息，很大程度上保证了数据的一致性，但会产生大量的日志。

### MIXED模式（MBR）
结合STATEMENT模式和ROW模式使用，优先使用STATEMENT模式保存binlog，对于STATEMENT模式无法保存的数据，则使用ROW模式进行保存。对于STATEMENT模式是否能够保存数据，由MySQL自行判断。

## 主从复制模式
### 全同步模式
主库执行完一个事务后，所有从库也执行该事务，当所有从库都执行完该事务后才返回给客户端。
优点：确保数据的一致性
缺点：性能影响严重

### 异步复制
此模式为默认。
主库执行完一个事务后，立即将更新数据发送给从库，并不关心从库是否接收。
优点：减少部分资源消耗
缺点：无法确认从库是否复制成功

### 半同步模式
相当于异步复制的改良版，主库执行完事务后，至少一个从库接收并写入到relay log中才返回给客户端。
优点：确保从库复制成功
缺点：这个过程需要花费一部分时间

## mysql 安装

- 物理机版 (出现 mysqld 没有问题)
```shell
wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
yum instal ./mysql80-community-release-el7-1.noarch.rpm 
yum install mysql-community-server

systemctl start mysql      //启动mysql 
systemctl status mysqld    //查看mysql状态 
systemctl enable mysqld //配置开机启动 

grep 'temporary password' /var/log/mysqld.log  //找到默认密码 
mysql -uroot -p  
setpassword for'root'@'localhost'=password('NEWPASSWORD'); 
# 或者
ALTERUSER 'root'@'localhost'IDENTIFIEDBY 'NEWPASSWORD';// 修改密码,注意密码要复杂一些，否则会不能通过。 
alter user user() identified by "XXXXXX"; 
```

- dockerk (有权限问题)
```shell
docker run -p 3306:3306 --name mysql \
    -v /data/docker/mysql/conf:/etc/mysql \
    -v /data/docker/mysql/logs:/var/log/mysql \
    -v /data/docker/mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    -d mysql:5.7
```

## 实践
```shell
# 主
create database replication;
use replication;
create table test(id int,name varchar(233));
insert into test values(1,'一主一从');

# 主：创建主从复制授权用户
create user 'slave'@'192.168.78.%' identified by '123456'; # 创建用户 指定网段
grant replication slave on *.* to 'slave'@'192.168.78.%';  # 授权 指定网段

# 主增加配置
server-id=1 # 服务器 id
binlog-format=MIXED # binlog 日志格式
log-bin=/data/mysql/log/mysql-bin-master # binlog 存放位置

# 主
show variables like "binlog_format";
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | MIXED |
+---------------+-------+

show master status
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000002 | 1817     |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+



# 从配置
server-id=2
relay-log=/data/mysql/log/relay-log-bin # 中继日志存放位置
relay-log-index=/data/mysql/log/slave-relay-bin.index # 中继日志索引存放位置

# 从
stop slave # 先关闭主从复制
change master to MASTER_HOST='192.168.78.213',MASTER_USER='slave',MASTER_PASSWORD='123456',MASTER_LOG_FILE='mysql-bin.000003',MASTER_LOG_POS=154; # 连接主库
# master_host：主库IP地址
# master_user：主库授权用户
# master_password：主库授权用户密码
# master_log_file：主库binlog日志文件名称
# master_log_pos：主库binlog日志文件位置
start slave # 开启主从复制

show slave status\G # 查询信息
...
Slave_IO_Running              | Yes
Slave_SQL_Running             | Yes
...
# 有一个不等于 YES 看
ERRNO


# 主
show processlist\G
***************************[ 1. row ]***************************
Id      | 17
User    | root
Host    | 192.168.122.114:45094
db      | <null>
Command | Query
Time    | 0
State   | starting
Info    | show processlist
***************************[ 2. row ]***************************
Id      | 20
User    | slave
Host    | 192.168.78.214:60742
db      | <null>
Command | Binlog Dump
Time    | 196
State   | Master has sent all binlog to slave; waiting for more updates
Info    | <null>

```

- 查询主备节点连接
```sql
# 需要在主节点且具备 root 权限
select HOST from information_schema.processlist as p where p.command like 'Binlog Dump%';
```

# GTID 方式 (代补充)

## 异常

- Got fatal error 1236 from master when reading data from binary log: 'Could not find first log file name in binary log index file'
```
change master to MASTER_HOST='192.168.78.213',MASTER_USER='slave',MASTER_PASSWORD='123456',MASTER_LOG_FILE='mysql-bin.000003',MASTER_LOG_POS=154; # 连接主库
应该是这句有问题
```
