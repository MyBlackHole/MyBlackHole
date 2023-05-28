# xtrabackup
mysql innodb备份工具

xtrabackup8 不再有 innodbackupex

## build 
```shell
# 需要 openssl 1.x.x
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_PREFIX_PATH=/media/black/Data/lib/openssl/openssl_1_1_1/ \
      -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/media/black/Data/lib/boost/boost_1_77_0/ ..

# 指定 gcc 7 (>=7.4) 与安装路径
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=/media/black/Data/lib/xtrabackup/xtrabackup_8_0/ \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_PREFIX_PATH=/media/black/Data/lib/openssl/openssl_1_1_1/ \
      -DCMAKE_CXX_COMPILER=/media/black/Data/lib/gcc/gcc_7_4_0/bin/g++ \
      -DCMAKE_C_COMPILER=/media/black/Data/lib/gcc/gcc_7_4_0/bin/gcc \
      -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/media/black/Data/lib/boost/boost_1_77_0/ ..
```

## 优点
1. 备份速度快
支持全量、增量备份
2. 备份过程不会打断正在执行的事物（无须锁表）
3. 能够基于压缩等功能节约磁盘空间和流量
4. 自动备份校验
5. 还原速度快
6. 可以流传将备份传递到另一台机器上
7. 在不增加服务负载的情况下备份数据？

## 原理
![[imgs/Pasted image 20230325184640.png]]

1. innobackupex启动后，会先fork一个进程，用于启动xtrabackup，然后等待xtrabackup备份ibd数据文件；
2. xtrabackup在备份innoDB数据是，有2种线程：redo拷贝线程和ibd数据拷贝线程。xtrabackup进程开始执行后，会启动一个redo拷贝的线程，用于从最新的checkpoint点开始顺序拷贝redo.log；再启动ibd数据拷贝线程，进行拷贝ibd数据。这里是先启动redo拷贝线程的。在此阶段，innobackupex进行处于等待状态（等待文件被创建）
4. xtrabackup拷贝完成ibd数据文件后，会通知innobackupex（通过创建文件），同时xtrabackup进入等待状态（redo线程依旧在拷贝redo.log）
5. innobackupex收到xtrabackup通知后哦，执行FLUSH TABLES WITH READ LOCK（FTWRL），取得一致性位点，然后开始备份非InnoDB文件（如frm、MYD、MYI、CSV、opt、par等格式的文件），在拷贝非InnoDB文件的过程当中，数据库处于全局只读状态。
6. 当innobackup拷贝完所有的非InnoDB文件后，会通知xtrabackup，通知完成后，进入等待状态；
7. xtrabackup收到innobackupex备份完成的通知后，会停止redo拷贝线程，然后通知innobackupex，redo.log文件拷贝完成；
8. innobackupex收到redo.log备份完成后，就进行解锁操作，执行：UNLOCK TABLES；
9. 最后innbackupex和xtrabackup进程各自释放资源，写备份元数据信息等，innobackupex等xtrabackup子进程结束后退出

### Xtrabackup增量备份原理
1. 首先完成一个完全备份，并记录下此时检查点LSN；

2. 然后增量备份时，比较表空间中每个页的LSN是否大于上次备份的LSN，若是则备份该页并记录当前检查点的LSN。

- 优点
1. 数据库太大没有足够的空间全量备份，增量备份能有效节省空间，并且效率高；
2. 支持热备份，备份过程不锁表（针对InnoDB而言），不阻塞数据库的读写；
3. 每日备份只产生少量数据，也可采用远程备份，节省本地空间；
4. 备份恢复基于文件操作，降低直接对数据库操作风险；
5. 备份效率更高，恢复效率更高。

## 安装
```
# centos
wget https://www.percona.com/downloads/XtraBackup/Percona-XtraBackup-2.4.9/binary/redhat/6/x86_64/Percona-XtraBackup-2.4.9-ra467167cdd4-el6-x86_64-bundle.tar

# ubuntu 
wget https://repo.percona.com/apt/percona-release_latest.generic_all.deb
sudo dpkg -i percona-release_latest.generic_all.deb
sudo apt update
sudo apt install percona-xtrabackup-80

# 源码编译 ??
git clone git@github.com:percona/percona-xtrabackup.git
mkdir debug
cd debug
cmake -DDOWNLOAD_BOOST=1 -DWITH_BOOST=dwith_boost -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -DCMAKE_BUILD_TYPE=Debug ..
make
```

## 案例
[详细]https://blog.csdn.net/dwjriver/article/details/117792271
- 备份
```shell
# 注意版本
sudo xtrabackup --defaults-file=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/config/my.cnf \
--datadir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/data/ \
--host='127.0.0.1' \
--user='root' \
--password='3edc#EDC' \
--port=3306 \
--backup \
--target-dir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/mysql_backup
--slave-info 在从库上备份的时候开启该参数，会生成一个xtrabakcup_slave_info，

该文件会记录备份完成的时候从库执行到主库的哪个位点。
这里顺便提及另外一个文件，xtrabackup_binlog_info，这个加不加该参数都会生成，
该文件记录备份的那台机器上的MySQL自己的binlog执行的位点位置
--compress 启用压缩功能，会将ibd文件压缩成.qp文件,压缩比我自己测试在1:12
--compress-threads=4  启动多少线程去执行压缩

--decompress 解压参数
--parallel 解压的线程数量


# 增量指定增量备份目录参数
sudo xtrabackup --defaults-file=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/config/my.cnf \
--datadir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/data/ \
--host='127.0.0.1' \
--user='root' \
--password='3edc#EDC' \
--port=3306 \
--backup \
--target-dir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/mysql_backup_in \
--incremental-basedir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/mysql_backup
```

- 还原
```shell
# 准备（恢复数据一致性）
sudo xtrabackup --defaults-file=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/config/my.cnf \
--prepare \
--target-dir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/mysql_backup

# 对增量准备 (最后一次增量准备不用 --apply-log-only)
# 先对全量准备
# 后对增量准备,--incremental-basedir 需按照增量备份顺序指定当时的增量目录 
sudo xtrabackup --defaults-file=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/config/my.cnf \
--prepare \
--apply-log-only \
--target-dir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/mysql_backup \
--incremental-basedir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/mysql_backup_in


# 修改 my.cnf 数据目录, 还原到此修改路径
# [mysqld]
# datadir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/data/
sudo xtrabackup --defaults-file=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/config/my.cnf \
--copy-back \
--target-dir=/media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/mysql_backup

# 还原数据目录权限与所属用户
chowm -R mysql:mysql /media/black/Data/Documents/github/CPP/percona-xtrabackup/Debug/docker_mysql/data/

# 启动数据库
```

- 流
```shell
由于xbstream+压缩备份后，无备份目录，xtrabackup可指定--extra-lsndir目录，此目录只存放此次备份的xtrabackup_checkpoints文件；后面的增量备份时，--incremental-basedir就指向前一日的extra-lsndir目录便可。
```
