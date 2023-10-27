# postgres
开源数据库

## 安装

### docker
```shell
docker run -it --name postgres --restart always -e POSTGRES_PASSWORD='abc123' -e ALLOW_IP_RANGE=0.0.0.0/0 -v /home/postgres/data:/var/lib/postgresql -p 55433:5432 -d postgres
<!-- –name : 自定义容器名称 -->
<!-- -e: 添加环境变量  -->
<!--     ALLOW_IP_RANGE=0.0.0.0/0，这个表示允许所有ip访问，如果不加，则非本机 ip 访问不了 -->
<!--     POSTGRES_PASSWORD：数据库密码 -->
<!-- -v :进行映射,本地目录：容器内路径 -->
<!-- -p：映射端口,宿主机端口：容器端口 -->
<!-- 最后是 镜像名称:端口号 -->
```

### ubuntu apt
```shell
sudo apt install postgresql postgresql-contrib
```

### 源码构建
```shell
sudo apt install libxml2-utils

# 检测配置项目
./configure --prefix=/opt/postgres15 --enable-debug
make -j16
sudo make install

# groupadd postgres
# useradd -g postgres postgres

# 运行
# 初始化数据库
./bin/initdb -D data

# 启动数据库 
./bin/pg_ctl -D data/ -l logfile start (后台)
./bin/postgres -D data/ (前台)

#启动数据库
pg_ctl -D /opt/postgresql/data start

#重新启动数据库
pg_ctl -D /opt/postgresql/data restart

#查看数据库状态
pg_ctl -D /opt/postgresql/data status

#停止数据库服务
pg_ctl -D /opt/postgresql/data stop
```

## 配置
```shell
# ubuntu
sudo su postgres
psql

## 修改默认账号密码
alter user postgres with password '1234';

## 创建用户
create user black with password '1234';

## 给新建用户管理员权限
alter user black with superuser;

## 查询账户信息
\du;

## black 可以 pgcli 登陆
pgcli -U black -d template1 -W

# 开启预处理事务
max_prepared_transactions = 100

# wal (预写日志)
synchronous_commit = on (默认值)
合法值 = local,remote_write,remote_apply,on,off

## on
1 为on且没有开启同步备库的时候,会当wal日志真正刷新到磁盘永久存储后才会返回客户端事务已提交成功,
2 当为on且开启了同步备库的时候(设置了synchronous_standby_names),必须要等事务日志刷新到本地磁盘,并且还要等远程备库也提交到磁盘才能返回客户端已经提交.

## off
写到缓存中就会向客户端返回提交成功，但也不是一直不刷到磁盘，延迟写入磁盘,延迟的时间为最大3倍的wal_writer_delay参数的(默认200ms)的时间,所有如果即使关闭synchronous_commit,也只会造成最多600ms的事务丢失,此事务甚至包括已经提交的事务（会丢数据）,但数据库确可以安全启动,不会发生块折断,只是丢失了部分数据,但对高并发的小事务系统来说,性能来说提升较大。

## local
当事务提交时,不仅要把wal刷新到磁盘,还需要等wal日志发送到备库操作系统(但不用等备库刷新到磁盘),因此如果备库此时发生实例中断不会有数据丢失,因为数据还在操作系统上,而如果操作系统故障,则此部分wal日志还没有来得及写入磁盘就会丢失,备库启动后还需要想主库索取wal日志。

## remote_write
当事务提交时,仅写入本地磁盘即可返回客户端事务提交成功,而不管是否有同步备库
```

## mvcc
1. oid 
    oid是object identifier的简写,其相关的参数设置default_with_oids设置一般默认是false,或者创建表时指定with (oids=false)，其值长度32bit,实际的数据库系统应用中并不能完全保证其唯一性; 
2. tableoid 
    是表对象的一个唯一标识符，可以和pg_class中的oid联合起来查看 
3. xmin 
    是插入的事务标识符,是用来标识不同事务下的一个版本控制。每一次更新该行都会改变这个值。可以和mvcc版本结合起来看 
4. xmax 
    是删除更新的事务标识符，如果该值不为0，则说明该行数据当前还未提交或回滚。比如设置begin事务时可以明显看到该值的变化 
5. cmin 
    插入事务的命令标识符,从0开始 
6. cmax 
    删除事务的命令标识符，或者为0 
7. ctid 
    是每行数据在表中的一个物理位置标识符，和oracle的rowid类似，但有一点不同，当表被vacuum full或该行值被update时该值可能会改变。所以定义表值的唯一性最好还是自己创建一个序列值的主键列来标识比较合适。不过使用该值去查询时速度还是非常快的。

