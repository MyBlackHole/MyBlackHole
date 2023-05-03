# mysql

## build
```shell
sudo apt-get install libldap2-dev ldap-utils
sudo apt-get install libsasl2-dev

# 需要 openssl 1.x.x
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_PREFIX_PATH=/media/black/Data/lib/openssl \
      -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/media/black/Data/lib/boost ..

# cmake -DCMAKE_BUILD_TYPE=Debug \
#       -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
#       -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
#       -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" \
#       -DWITH_SSL=/media/black/Data/lib/openssl \
#       -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/media/black/Data/lib/boost ..

# cmake -DCMAKE_BUILD_TYPE=Debug \
#       -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
#       -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
#       -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" \
#       -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/media/black/Data/lib/boost ..
```

## 配置
```sql
show variables like '%data%';
```

## 文件详解

- myisam
1. *.frm--表定义，是描述表结构的文件。
2. *.MYD--"D"数据信息文件，是表的数据文件。
3. *.MYI--"I"索引信息文件，是表数据文件中任何索引的数据树。

- innodb
1. *.frm--表定义，是描述表结构的文件。
2. *.ibd--表数据和索引的文件。该表的索引(B+树)的每个非叶子节点存储索引，叶子节点存储索引和索引对应的数据


## 安装、权限、用户
- docker 版
```shell
# 拉去镜像
docker pull mysql

# 启动
```shell
# 异常
#sudo chowm 999:999 data conf logs
docker run -p 3306:3306 --name mysql \
    --privileged=true \
    -v /etc/localtime:/etc/localtime:ro \
    -v /opt/docker/mysql/conf:/etc/mysql \
    -v /opt/docker/mysql/logs:/var/log/mysql \
    -v /opt/docker/mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=p@3Sw0rd \
    -d mysql:5.7

# 修改data、logs、conf 用户与用户组为 999:999
#sudo chowm 999:999 data conf logs
docker run -p 3306:3306 --name mysql \
    -v /etc/localtime:/etc/localtime:ro \
    -v $(pwd)/test_mysql/conf:/etc/mysql \
    -v $(pwd)/test_mysql/logs:/var/log/mysql \
    -v $(pwd)/test_mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    -d mysql

# 或
docker run -it --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag 

# 或

mkdir -p /opt/docker/mysql/conf/conf.d
docker run -p 3306:3306 --name mysql \
    -v /etc/localtime:/etc/localtime:ro \
    -v /opt/docker/mysql/conf:/etc/mysql \
    -v /opt/docker/mysql/logs:/var/log/mysql \
    -v /opt/docker/mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=p@3Sw0rd \
    -d mysql:8
```

- 物理机版
```shell
# centos
## 5.7
wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm

## 8.0
wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm 

sudo yum instal ./mysql80-community-release-el7-1.noarch.rpm 

yum install mysql-community-server.x86_64 
sudo service mysqld start      //启动mysql 
sudo service mysqld status   //查看mysql状态 
sudo systemctl enable mysqld //配置开机启动 
grep 'temporary password' /var/log/mysqld.log  //找到默认密码 
mysql -uroot -p  
setpassword for'root'@'localhost'=password('NEWPASSWORD'); 
或者ALTERUSER 'root'@'localhost'IDENTIFIEDBY 'NEWPASSWORD';// 修改密码,注意密码要复杂一些，否则会不能通过。 
alter user user()identified by "XXXXXX"; 

# ubuntu
sudo apt-get update 
sudo apt-get install mysql-server 
sudo apt install mysql-workbench（可视化界面） 
service mysql start 
service mysql stop 

# win
https://cdn.mysql.com/Downloads/MySQL-8.0/mysql-8.0.26-winx64.msi 
```

- 源码编译
```shell
mkdir bulid
cd bulid
cmake -DDOWNLOAD_BOOST=1 -DWITH_BOOST=dwith_boost -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -DCMAKE_BUILD_TYPE=Debug ..
make
sudo make install
```

- 用户配置
创建用户，授予远程登录权限、操作所有数据权限
create USER BlackHole@'%' identified by '1358244533';
grant all on *.* to BlackHole@'%' with grant option;
flush privileges;

- 授权
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

```sql
# 例如
# 8.0前
grant all privileges on minidome.* to Black@'%' identified by '1358244533';  # 授权Blcak具有远程登陆与minidome数据库的所有权限 
# 8.0后
create USER BlackHole@'%' identified by '1358244533';  # 创建具有远程登陆的BlackHole用户 
grant all on minidome.* to BlackHole@'%' with grant option;  # 授予BlackHole具有minidome的所有权限 
flush privileges; # 表示刷新权限变更 
show grants for 'heidong'; # 查看权限授予
```

- 创建账号
```sql
create USER BlackHole@'%' identified by '1358244533';  # 创建具有远程登陆的BlackHole用户
```

- 删除
```sql
drop user BlackHole@'localhost';  # 删除用户及权限 
drop database minidome; # 删除数据库
Delete FROM user Where User='Black' and Host='localhost'  # 删除用户
```

- 更新
```sql
update mysql.user set password=password('新密码') where User="BlackHole" and Host="localhost"; # 更新密码
```

- 查看表结构
```shell
# 基本结构
describe test2;
# 详细结构
show create table test\G
```

- 查看表索引
```shell
show index from task_instance;
```

## 命令
- 查看正在连接会话
```sql
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
```sql
SELECT * from information_schema.INNODB_TRX;
select concat('KILL ',id,';') from information_schema.processlist p inner join information_schema.INNODB_TRX x on p.id=x.trx_mysql_thread_id where db='user';
```

- 获取活动的锁
```sql
SELECT * FROM information_schema.INNODB_LOCKs;
```

- 锁等待的对应关系
```sql
mysql> SELECT * FROM information_schema.INNODB_LOCK_waits;
```

- 表加读锁
```sql
lock tables user read;
```

- 权限查询
```shell
show grants

# 指定账户
show grants for 'root'@'%'
```

## 连接 mysql
```shell
mysql -h 127.0.0.1 -P 3306 -uroot –p 
```

## 例子

