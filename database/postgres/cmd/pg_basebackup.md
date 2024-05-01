# pg_basebackup

## help
```shell
postgres@s2ahumysqlpg01-> pg_basebackup --help 
用法：
  pg_basebackup [选项] ...

控制输出的选项：
  -D, --pgdata=DIRECTORY  接收基本备份到目录
  -F, --format=p|t                     输出格式（plain（默认），tar）
  -r, --max-rate=RATE             传输数据目录的最大传输速率（以 kB/s 为单位，或使用后缀“k”或“M”）
  -R, --write-recovery-conf       用于复制的写入配置
  -T, --tablespace-mapping=OLDDIR=NEWDIR 将 OLDDIR 中的表空间重定位到 NEWDIR
      --waldir=WALDIR             预写日志目录的位置
  -X, --wal-method=none|fetch|stream 包含指定方法所需的 WAL 文件
  -z, --gzip                               压缩 tar 输出
  -Z, --compress=0-9               使用给定的压缩级别压缩 tar 输出

常规选项：
  -c, --checkpoint=fast|spread 设置快速或扩展检查点
  -C, --create-slot 创建复制槽
  -l, --label=LABEL 设置备份标签
  -n, --no-clean 出错后不清理
  -N, --no-sync 不等待更改安全写入磁盘
  -P, --progress 显示进度信息
  -S, --slot=SLOTNAME 要使用的复制槽
  -v, --verbose 输出详细信息
  -V, --version 输出版本信息，然后退出
      --no-slot 防止创建临时复制槽
      --no-verify-checksums 不验证校验和
  -?, --help 显示此帮助，然后退出

连接选项：
  -d, --dbname=CONNSTR 连接字符串
  -h, --host=HOSTNAME 数据库服务器主机或套接字目录
  -p, --port=PORT 数据库服务器端口号
  -s, --status-interval=状态包发送到服务器的间隔时间（以秒为单位）
  -U, --username=NAME 以指定的数据库用户连接
  -w, --no-password 从不提示输入密码
  -W, --password 强制密码提示（应该自动
```


## 备份

### 开启备份
```shell
创建归档目录
mkdir -p /ssd/pg957/arch
chown -R postgres:postgres /ssd/pg957/arch

配置归档命令
vi $PGDATA/postgresql.conf
archive_mode = on
archive_command = 'DATE=`date +%Y%m%d`; DIR="/ssd/pg957/arch/$DATE"; (test -d $DIR || mkdir -p $DIR) && cp %p $DIR/%f'
wal_level = hot_standby  

# 备注说明
注释：
%p 表示xlog文件名$PGDATA的相对路径, 如pg_xlog/00000001000000190000007D
%f 表示xlog文件名, 如00000001000000190000007D

备注：
wal_level：指定生成wal日志的级别，值为minmal，archive，hot_standby。
minmal一般的配置，archive会生成wal归档需要的日志记录，hot_standby添加备库时需要设置。 
```


### 重启验证归档
```shell
checkpoint;                #   解释checkpoint做了什么
select pg_switch_wal() ;   # 10g 以前用 select pg_switch_xlog();     

[root@hgdb01 20180117]# pwd
/ssd/pg957/arch/20180117
[root@hgdb01 20180117]# ls
000000020000000000000003  000000020000000000000004
```

### 创建 replication 权限角色
```shell
创建replication权限的角色, 或者超级用户的角色。
create role repl nosuperuser replication login connection limit 32 encrypted password '111111';
```

### 配置 pg_hba.conf
```shell
配置pg_hba.conf，添加以下内容
host replication repl 0.0.0.0/0 md5
```


### 执行备份
```shell
#因为使用流复制协议, 所以支持异地备份
pg_ctl reload  #执行加载配置的命令
mkdir `date +%F` ; 
pg_basebackup -F t -x -D /home/black/bak/`date +%F` -h 127.0.0.1 -p 5433 -U postgres

V12：
pg_basebackup -F t -X s -D /home/black/bak/`date +%F` -h 127.0.0.1 -p 5433 -U postgres
```

### 备份检查
```shell
备份完毕，查看备份文件
cd /home/black/bak/
ll
总计 39M
-rw------- 1 black black 134K 11月 25 17:18 backup_manifest
-rw------- 1 black black  23M 11月 25 17:18 base.tar
-rw------- 1 black black  17M 11月 25 17:18 pg_wal.tar
tar -tvf base.tar |less       #查看备份包内容


pg_basebackup 备份参数 -F -t -X
   -F 指定了输出的格式，支持p（原样输出）或者t（tar格式输出）。
   -X 表示备份开始后，启动另一个流复制连接从主库接收WAL日志。

   16400表空间的oid
   简单讲述表空间的软连接
[postgres@hgdb01 pg_tblspc]$ ls -l
    total 0
    lrwxrwxrwx. 1 postgres postgres 14 Jan 17 01:04 16400 -> /pgtbls/tbls01     
```


## 恢复


### 例子
```shell
pg_basebackup -D /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06 -Ft -U black -h 127.0.0.1 -p 5432 -P


/run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06
❯ ls -alh
Permissions Size User  Date Modified Name
.rw-------  138k black  6 Jan 11:56   backup_manifest
.rw-------   24M black  6 Jan 11:56   base.tar
.rw-------   17M black  6 Jan 11:56   pg_wal.tar

cd /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/
mkdir /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data
tar xvf base.tar -C /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data/

chmod -R 700 /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data

mkdir /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/pg_wal
tar xvf pg_wal.tar -C /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/pg_wal/

lvim /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data/postgresql.conf
restore_command = 'cp /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/pg_wal/%f %p'

touch /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data/recovery.signal

# 启动数据库
<!-- ./src/backend/postgres -D /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data -p 5432 -h 127.0.0.1 -c config_file=/run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data/postgresql.conf -->
pg_ctl -D /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data -l /run/media/black/Data/Documents/github/C/postgres/Debug/buackup/2024-01-06/data/pg_log start
```

### 切换日志
```shell
在需要备份的库中创建标记表，并检查点和归档指令
    create table t_flag(id int) ;
    insert into t_flag values(1);
    checkpoint;        #刷新内存脏页到磁盘
    select pg_switch_wal();    #手动日志归档
```

### 删除原数据
```shell
停止数据库并删除数据目录，将pg_basebackup生成的备份包分别解压到相应目录
pg_ctl stop -D /home/black/bak/postgresql
清空 data_bak 里的文件 
rm -rf  /home/black/bak/postgresql/*

# 解压备份文件到指定目录 
tar -xvf base.tar -C  /home/black/bak/postgresql/data/

# 如果归档文件存在可以直接用归档文件 
tar -xvf pg_wal.tar.gz -C  /home/black/bak/postgresql/pg_wal/
```

### 恢复参数
```shell
<!-- pg12以前 -->
在PG12以前需要修改 recovery.conf文件配置还原参数
cp $PGHOME/share/recovery.conf.sample $PGDATA/recovery.conf 
vi $PGDATA/recovery.conf

# 配置 recovery_target_timeline 参数, 便于判断是否已经到达还原点. (可选, 仅做PITR时需要.一般都是恢复到最新)
# 对路径 /ssd/pg957/arch/20180118/ 的解释
# 备份完成后 recovery.conf 文件名会自动修改为 recovery.done
restore_command = 'cp /ssd/pg957/arch/20180118/%f %p'
recovery_target_timeline = 'latest'

<!-- pg12以后 -->
从v12开始，针对此问题进行了改进，把 recovery.conf 中的参数合到了 postgresql.conf 配置文件中，但在非恢复模式这些参数将被忽略。

从PG12开始，recovery.conf 文件不存在，由下面两个新文件进行替换：
recovery.signal：告诉 PostgreSQL 进入正常的归档恢复
standby.signal：告诉 PostgreSQL 进入 standby 模式
如果两个文件都存在，则standby.signal优先。


export PGDATA=/home/black/bak/postgresql/data

# 告诉PostgreSQL进入正常的归档恢复
touch $PGDATA/recovery.signal

echo "
restore_command = 'cp /home/black/bak/postgresql/pg_wal/%f    %p'
recovery_target = 'immediate'
"  >> $PGDATA/postgresql.auto.conf
```

### 启动验证
```shell
pg_ctl start -D /home/black/bak/postgresql/data
```


## 基于时间点恢复
```shell
默认情况下，恢复将恢复到 WAL 日志的末尾即，备份那一刻的日志。即recovery_target默认为immediate。
以下参数可用于指定较早的停止点。最多可以使用recovery_target
, recovery_target_lsn
, recovery_target_name
, recovery_target_time
, or之一；recovery_target_xid
如果在配置文件中指定了其中一个以上，则会引发错误。这些参数只能在服务器启动时设置。https://www.postgresql.org/docs/14/runtime-config-wal.html

recovery_target = ’immediate’指定恢复应该在达到一个一致状态后尽快结束，即尽早结束。 在从一个在线备份中恢复时，这意味着备份结束的那个点。
recovery_target_name (string)指定恢复将继续进行的已命名的恢复点 （pg_create_restore_point()创建）。
recovery_target_time (timestamp)这个参数指定恢复将继续执行的时间戳。精确的停止点也受到recovery_target_inclusive的影响。
recovery_target_xid (string)指定恢复将继续执行的事务ID。
recovery_target_inclusive (boolean)指定我们是否在指定的恢复目标之后停止（true）， 或者在恢复目标之前停止（false）。
recovery_target_timeline (string)指定恢复到一个特定的时间线中。默认值是沿着基础备份建立时的当前时间线恢复。
将这个参数设置为latest会恢复到该归档中能找到的最新的时间线， 这在一个后备服务器中有用。
recovery_target_action (enum) (boolean)指定当到达恢复目标时服务器应该采取什么动作。默认值是pause， 这意味着将暂停恢复。
promote意味着将结束恢复进程并且服务器开始接受连接。 shutdown将在到达恢复目标后停止服务器。
```

### 示例
基于时间点注意事项：
1. 归档日志完整
2. 指定从归档目录万利 restore_command = 'cp /u01/postgresql/arch/%f %p'
3. 设置recovery_target_time 或 recovery_target_lsn 或 recovery_target_xid
4. 创建touch $PGDATA/recovery.signal
```shell
# 示例： 
# 设置基于时间的恢复 
recovery_target_time = '2022-02-07 13:45:00+08'   

#设置基于lsn,或 xid 的恢复
recovery_target_lsn  或 recovery_target_xid  可以通过pg_waldump 解析日志，来确定恢复目标
如本例需要恢复到COMMIT  可以设置 xid 为 '34'  或 lsn 为 '0/DA0000F8'
recovery_target_xid='34' 或  recovery_target_lsn='0/DA0000F8'

postgres@s2ahumysqlpg01-> pg_waldump 0000000400000000000000DA

mgr: Standby     len (rec/tot):     50/    50, tx:          0, lsn: 0/DA000028, prev 0/D9000100, desc: RUNNING_XACTS nextXid 2173559 latestCompletedXid 2173558 oldestRunningXid 2173559
rmgr: Heap        len (rec/tot):     54/   150, tx:    2173559, lsn: 0/DA000060, prev 0/DA000028, desc: INSERT off 2 flags 0x00, blkref #0: rel 1663/13548/34369 blk 0 FPW
rmgr: Transaction len (rec/tot):     34/    34, tx:    2173559, lsn: 0/DA0000F8, prev 0/DA000060, desc: COMMIT 2022-02-21 17:40:31.486619 HKT
rmgr: Standby     len (rec/tot):     50/    50, tx:          0, lsn: 0/DA000120, prev 0/DA0000F8, desc: RUNNING_XACTS nextXid 2173560 latestCompletedXid 2173559 oldestRunningXid 2173560
rmgr: XLOG        len (rec/tot):     24/    24, tx:          0, lsn: 0/DA000158, prev 0/DA000120, desc: SWITCH 
```

### 恢复控制
```shell
pg_is_wal_replay_paused() → boolean         如果请求恢复暂停，则返回 true。
pg_get_wal_replay_pause_state() → text      返回恢复暂停状态。返回值是not paused是否未请求pause requested暂停，是否请求暂停但恢复尚未暂停，以及paused恢复是否实际暂停。
pg_promote ( wait boolean DEFAULT true, wait_seconds integer DEFAULT 60 ) → boolean   
                                            将备用服务器提升为主状态。wait如果设置为（默认值），该true函数会等待升级完成或wait_seconds秒数过去，true如果升级成功则返回，false否则返回。如果wait设置为false，则函数在向 postmastertrue发送SIGUSR1信号以触发提升后立即返回。
                                             默认情况下，此功能仅限超级用户使用，但可以授予其他用户 EXECUTE 权限来运行该功能。
pg_wal_replay_pause() → void       请求暂停恢复。请求并不意味着恢复立即停止。如果要保证恢复实际上已暂停，则需要检查由pg_get_wal_replay_pause_state().    请注意，pg_is_wal_replay_paused()返回是否发出请求。恢复暂停时，不会应用进一步的数据库更改。如果热备用处于活动状态，所有新查询将看到相同的数据库快照，并且在恢复恢复之前不会产生进一步的查询冲突。
                                 默认情况下，此功能仅限超级用户使用，但可以授予其他用户 EXECUTE 权限来运行该功能。

pg_wal_replay_resume() → void 如果已暂停，则重新启动恢复。 默认情况下，此功能仅限超级用户使用，但可以授予其他用户 EXECUTE 权限来运行该功能。

注意：
    pg_wal_replay_pause 和 pg_wal_replay_resume不能在提升主库时进行执行。如果在恢复暂停时触发了提升，则暂停状态结束并继续提升。
    在pg 14中备份进行可以通过下面这个视图查询
    pg_stat_progress_basebackup
```
