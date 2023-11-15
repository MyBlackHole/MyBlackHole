# config

## 默认密码
```shell
<!-- mysql.log -->
2023-11-13T16:41:57.887704+08:00 1 [Note] A temporary password is generated for root@localhost: V#aX1h3foMi_

_myoldpass_=V#aX1h3foMi_

<!-- 初始化为自定义密码 -->
mysqladmin -uroot -p"${_myoldpass_}" password ${_mynewpass_}


- 修改外部可连接
cat > /tmp/my.sql <<EOF
use mysql;
ALTER USER 'root'@'localhost' IDENTIFIED BY '${_mynewpass_}';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '${_mynewpass_}' WITH GRANT OPTION;
SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
flush privileges;
EOF

mysql -h 127.0.0.1 -u root -pp@3Sw0rd < /tmp/my.sql

mysql> use mysql;
Database changed
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '${_mynewpass_}';
Query OK, 0 rows affected (0.02 sec)

mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '${_mynewpass_}' WITH GRANT OPTION;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
+------------------------------------+
| query                              |
+------------------------------------+
| User: 'root'@'%';                  |
| User: 'mysql.session'@'localhost'; |
| User: 'mysql.sys'@'localhost';     |
| User: 'root'@'localhost';          |
+------------------------------------+
4 rows in set (0.01 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

### mysql配置



```shell
[mysqld] 
server-id=513306                        # Mysql唯一标识，一个集群中唯一； 
port=3306                               # 服务端口，默认3306 
user = mysql                            # 启动用户，建议用户mysql 
bind_address= 0.0.0.0                   # 绑定的IP地址，建议使用具体地址 ::(tcp6\tcp4)
basedir=/mysql/app/mysql                # mysql安装路径，建议使用绝对路径 
datadir=/mysql/data/3306/data           # 数据目录 
socket=/mysql/data/3306/mysql.sock      # 指定套接字文件 
pid-file=/mysql/data/3306/mysql.pid     # 指定pid文件 
character-set-server=utf8               # 指定默认编码格式 
skip-character-set-client-handshake=1   # 跳过mysql程序起动时的字符参数设置 ，使用服务器端字符集设置 0:不跳过  1：跳过 
autocommit = 0                          # 是否开启自动提交， 0:不开启 1：开启 
skip_name_resolve = 1                   # 禁止域名解析   建议开启 
max_connections = 800                   # 最大连接数 
max_connect_errors = 1000               # 最大连接错误 
default-storage-engine=INNODB           # 设置默认引擎，常用引擎INNODB，MYISAN，建议使用INNODB 
transaction_isolation = READ-COMMITTED  # 事务隔离级别，可选参数有：READ-UNCOMMITTED(读取未提交内容), READ-COMMITTED（读取提交内容）, REPEATABLE-READ(可重读), SERIALIZABLE(可串行化). 
explicit_defaults_for_timestamp = 1     # 参数是否初始化 
sort_buffer_size = 32M                  # 排序使用的缓存大小，MySQL5.7中，默认为1M(优化参数之一，一般情况下默认数值就够用了) 
join_buffer_size = 128M                 # join操作所用用的缓存大小 
tmp_table_size = 72M                    # 临时表大小 
max_allowed_packet = 16M                # 服务端最大允许接收的数据包大小 
sql_mode = "STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER"   # mysql支持的基本语法及校验规则 
interactive_timeout = 1800              # 是MySQL在等待一个活动连接关闭连接前等待的秒数。默认28800秒，8小时 
wait_timeout = 1800                     # 是MySQL在等待一个非活动连接关闭连接前等待的秒数。默认28800秒，8小时 
read_buffer_size = 16M                  # （数据文件存储顺序）是MySQL读入缓冲区的大小，将对表进行顺序扫描的请求将分配一个读入缓冲区，MySQL会为它分配一段内存缓冲区，read_buffer_size变量控制这一缓冲区的大小，如果对表的顺序扫描非常频繁，并你认为频繁扫描进行的太慢，可以通过增加该变量值以及内存缓冲区大小提高其性能，read_buffer_size变量控制这一提高表的顺序扫描的效率 数据文件顺序。 
read_rnd_buffer_size = 32M              # 是MySQL的随机读缓冲区大小，当按任意顺序读取行时（列如按照排序顺序）将分配一个随机读取缓冲区，进行排序查询时，MySQL会首先扫描一遍该缓冲，以避免磁盘搜索，提高查询速度，如果需要大量数据可适当的调整该值，但MySQL会为每个客户连接分配该缓冲区所以尽量适当设置该值，以免内存开销过大。表的随机的顺序缓冲 提高读取的效率。 
#event_scheduler =1　　　　　　　　　　　　 # 事件调度器  1：开启   0:不开启 
query_cache_type = 1                    # 控制着查询缓存工能的开启的关闭。0时表示关闭，1时表示打开，2表示只要select 中明确指定SQL_CACHE才缓存。 
query_cache_size=1M                     # 查询缓存大小， 一般 1M很够用了 
table_open_cache=2048                   # 文件描述符的缓存大小，4G内存的机器，建议设置为2048， 
thread_cache_size=768                   # 线程池缓存大小，当客户端断开连接后 将当前线程缓存起来 当在接到新的连接请求时快速响应 无需创建新的线程 
myisam_max_sort_file_size=10G           #  mysql重建索引时允许使用的临时文件最大大小 
myisam_sort_buffer_size=64M             # MyISAM表发生变化时重新排序所需的缓冲。一般64M足矣。 
key_buffer_size=32M                     #  Key Buffer大小，用于缓存MyISAM表的索引块。决定数据库索引处理的速度（尤其是索引读），对于内存在4GB左右的服务器该参数可设置为256M或384M。注意：该参数值设置的过大反而会是服务器整体效率降低！ 
read_buffer_size=8M                     # 用于对MyISAM表全表扫描时使用的缓冲区大小。针对每个线程进行分配（前提是进行了全表扫描）。进行排序查询时，MySql会首先扫描一遍该缓冲，以避免磁盘搜索，提高查询速度，如果需要排序大量数据，可适当调高该值。但MySql会为每个客户连接发放该缓冲空间，所以应尽量适当设置该值，以避免内存开销过大。 
read_rnd_buffer_size=4M                 # 是MySQL的随机读缓冲区大小，当按任意顺序读取行时（列如按照排序顺序）将分配一个随机读取缓冲区，进行排序查询时，MySQL会首先扫描一遍该缓冲，以避免磁盘搜索，提高查询速度，如果需要大量数据可适当的调整该值，但MySQL会为每个客户连接分配该缓冲区所以尽量适当设置该值，以免内存开销过大。表的随机的顺序缓冲 提高读取的效率。 
back_log=1024                           # 值指出在MySQL暂时停止回答新请求之前的短时间内多少个请求可以被存在堆栈中。由默认的50，每个连接256kb 
#flush_time=0 
open_files_limit=65536                  # MySQL打开了多少个文件描述符，默认最小1024 
table_definition_cache=1400             # 表定义文件缓存相比表文件描述符缓存所消耗的内存更小，其默认值是400 
#binlog_row_event_max_size=8K 
# 有时候为了避免master.info和中继日志崩溃，在容忍额外的fsync()带来的开销，推荐设置 
#sync_master_info=10000                 # 默认为10000，每间隔多少事务刷新master.info，如果是table（innodb）设置无效，每个事务都会更新，建议 设置为1 
#sync_relay_log=10000                   # 默认为10000，即每10000次sync_relay_log事件会刷新到磁盘。为0则表示不刷新，交由OS的cache控制，建议设置为1 
#sync_relay_log_info=10000              # 默认为10000，每间隔多少事务刷新relay-log.info，建议设置为1 

########log settings######## 
log-output=FILE                         # 日志存储方式，TABLE、FILE，建议设置为FILE，默认为FILE 
general_log = 0                         # 所有到达MySQL Server的SQL语句记录下来。通用日志，不建议开启，这个很消耗磁盘空间，用于优化及故障排查 
general_log_file=/mysql/log/3306/itpuxdb-general.err    # 指定通用日志文件 
slow_query_log = ON                     # ON 为开启慢查询日志，off表示关闭慢查询日志，用于优化SQL语句 
slow_query_log_file=/mysql/log/3306/itpuxdb-query.err   #指定慢查询日志文件 
long_query_time=10                      # 指定多少秒返回查询的结果为慢查询 
log-error=/mysql/log/3306/itpuxdb-error.err     # 指定错误日志 

log_queries_not_using_indexes = 1       # 开启 记录没有使用索引查询语句，1或者ON开启，记录至慢日志中 
log_slow_admin_statements = 1           #记录那些慢的optimize table，analyze table和alter table语句，1或者ON开启，记录至慢日志中 
log_slow_slave_statements = 1           # 记录由Slave所产生的慢查询 
log_throttle_queries_not_using_indexes = 10     # 设定每分钟记录到日志的未使用索引的语句数目，超过这个数目后只记录语句数量和花费的总时间  
expire_logs_days = 90                   # 保留多少天 
min_examined_row_limit = 100            # 对于查询扫描行数小于此参数的SQL，将不会记录到慢查询日志中 
#log_bin = "/log/bin_log/binlog"        # bin 日志路径设置 

########replication settings########    # 主从复制设置 
#master_info_repository = TABLE         # 值如果为FILE，建议将其修改为TABLE 
#relay_log_info_repository = TABLE 
#log_bin = bin.log 
#sync_binlog = 1　　　　　　　　　　　　　　 # 默认，sync_binlog=0，表示MySQL不控制binlog的刷新，如果sync_binlog>0，表示每sync_binlog次事务提交，# sync_binlog=1了，表示每次事务提交，MySQL都会把binlog刷下去，是最安全但是性能损耗最大的设置。sync_binlog=1，多个事务同时提交，同样很大的影响MySQL和IO性能。# MySQL DBA设置的sync_binlog并不是最安全的1，而是100或者是0。这样牺牲一定的一致性，可以获得更高的并发和性能。 
#gtid_mode = on                         # 是否开启开启 基于 gtid 的复制， 5.7之后才出现的新特性 
#enforce_gtid_consistency = 1           #  
#log_slave_updates 
#binlog_format = row  
#relay_log = relay.log 
#relay_log_recovery = 1 
#binlog_gtid_simple_recovery = 1 
#slave_skip_errors = ddl_exist_errors 

########innodb settings######## 
# 根据您的服务器IOPS能力适当调整,只有当你在频繁写操作的时候才有意义 
# 一般配普通SSD盘的话，可以调整到 10000 - 20000 
# 配置高端PCIe SSD卡的话，则可以调整的更高，比如 50000 - 80000  
innodb_io_capacity = 4000               # 动态调整刷新脏页的数量，一般设置最大值的1/2 
innodb_io_capacity_max = 8000           #动态调整刷新脏页的最大数量 
innodb_buffer_pool_size = 200M          # 缓存池大小，默认128M，建议设置为总内存大小的，设置为物理内存的80% 
innodb_buffer_pool_instances = 8        # 可以开启多个内存缓冲池，把需要缓冲的数据hash到不同的缓冲池中，这样可以并行的内存读写。 
innodb_buffer_pool_load_at_startup = 1  # 默认为关闭OFF。如果开启该参数，启动MySQL服务时，MySQL将本地热数据加载到InnoDB缓冲池中。 
innodb_buffer_pool_dump_at_shutdown = 1 # 默认启用。指定在MySQL服务器关闭时是否记录在InnoDB缓冲池中缓存的页面，以便在下次重新启动时缩短预热过程。 
innodb_lru_scan_depth = 2000            # LRU列表中可用页的数量，默认值为1024。 
innodb_lock_wait_timeout = 5            # 事务锁超时时间  
#innodb_flush_method = O_DIRECT 

innodb_log_file_size = 200M             # mysql事务日志文件（ib_logfile0）的大小； 
innodb_log_files_in_group = 2           # 指定你有几个日志组。一般２-３个日值组。默认为两个。 
innodb_log_buffer_size = 16M            # 事务在内存中的缓冲大小。 

innodb_undo_logs = 128                  # InnoDB使用的回滚段个数，必须设置35个以上；，默认128 
innodb_undo_tablespaces = 3             # 是控制undo是否开启独立的表空间的参数，为0表示：undo使用系统表空间，即ibdata1，不为0表示：使用独立的表空间，一般名称为 undo001 undo002，存放地址的配置项为：innodb_undo_directory，默认配置为0，参数必须大于或等于2，即回收（收缩）一个undo log日志文件时，要保证另一个undo log是可用的。 
innodb_undo_log_truncate = 1            # 参数设置为1，即开启在线回收（收缩）undo log日志文件，支持动态设置。 
innodb_max_undo_log_size = 2G           # 每一个undo日志文件大小，默认1G 

innodb_flush_neighbors = 1              # 刷脏页的控制策略，参数就是用来控制这个行为的，值为 1 的时候会有上述的“连坐”机制，值为 0 时表示不找邻居，自己刷自己的。建议设置为0 
innodb_purge_threads = 4                # 负责回收已经使用并分配的undo页，可以指定多个innodb_purge_threads来进一步加快和提高undo回收速度。 
innodb_large_prefix = 1                 # 单索引限制，是否开启允许列索引最大达到3072，不开启只有767 
innodb_thread_concurrency = 64          # 来限制并发线程的数量 
innodb_print_all_deadlocks = 1          # 是否开启保存死锁日志，死锁日志存放到error_log配置的文件里面 
innodb_strict_mode = 1                  # InnoDB严格检查模式，尤其采用了页数据压缩功能后，最好是开启该功能。开启此功能后，当创建表（CREATE TABLE）、更改表（ALTER TABLE）和创建索引（CREATE INDEX）语句时，如果写法有错误，不会有警告信息，而是直接抛出错误，这样就可直接将问题扼杀在摇篮 
innodb_sort_buffer_size = 64M 
innodb_flush_log_at_trx_commit=1        # sync_binlog 两个参数是控制MySQL 磁盘写入策略以及数据安全性的关键参数，当两个参数都设置为1的时候写入性能最差，但安全性最高， 
# 设置为0，log buffer将每秒一次地写入log file中，并且log file的flush(刷到磁盘)操作同时进行.该模式下，在事务提交的时候，不会主动触发写入磁盘的操作。 
# 设置为1，每次事务提交时MySQL都会把log buffer的数据写入log file，并且flush(刷到磁盘)中去. 
# 设置为2，每次事务提交时MySQL都会把log buffer的数据写入log file.但是flush(刷到磁盘)操作并不会同时进行。该模式下,MySQL会每秒执行一次 flush(刷到磁盘)操作。 
innodb_autoextend_increment=64          # 默认 8M ,单位为M，配置表空间自动扩展，每次扩展多大M 
innodb_concurrency_tickets=5000         # 这个参数设置为一种tickets,默认是5000， 
innodb_old_blocks_time=1000             # 页读取到mid位置后，需要等待多久才会被加入到LRU列表的热端。默认1000ms 
innodb_open_files=65536                 # 限制Innodb能打开的表的数据。 
innodb_stats_on_metadata=0              # 是否自动更新统计信息,默认为0关闭， 
innodb_file_per_table=1                 # MySQL InnoDB引擎 默认会将所有的数据库InnoDB引擎的表数据存储在一个共享空间中：ibdata1，当增删数据库的时候，ibdata1文件不会自动收缩，单个数据库的备份也将成为问题。通常只能将数据使用mysqldump 导出，然后再导入解决这个问题。如果启用了innodb_file_per_talbe参数，需要注意的是每张表的表空间内存放的只是数据、索引和插入缓冲Bitmap页，其他数据如：回滚信息、插入缓冲索引页、系统事物信息、二次写缓冲（Double write buffer）等还是放在原来的共享表空间内。同时说明了一个问题：即使启用了innodb_file_per_table参数共享表空间还是会不断的增加其大小的。 
#innodb_checksum_algorithm=0            # 是否开启checksum算法 
innodb_data_file_path=ibdata1:200M;ibdata2:200M;ibdata3:200M:autoextend:max:5G      #可配置表空间相关参数。 
innodb_temp_data_file_path = ibtmp1:200M:autoextend:max:20G         # 可配置临时表空间相关参数。 
 
innodb_buffer_pool_dump_pct = 40        # 指定每个缓冲池最近使用的页面读取和转储的百分比，1-100，默认25 
innodb_page_cleaners = 4                # 多个页面清理线程刷脏页，用于指定页面清理线程的数量。其默认值1，维持了之前单个页面清理线程的配置 
innodb_purge_rseg_truncate_frequency = 128      # 指定purge操作被唤起多少次之后才释放rollback segments。当undo表空间里面的rollback segments被释放时，undo表空间才会被truncate。由此可见，该参数越小，undo表空间被尝试truncate的频率越高。 
binlog_gtid_simple_recovery=1           #这个变量用于在MySQL重启或启动的时候寻找GTIDs过程中，控制binlog 如何遍历的算法？ 
#2. 当binlog_gtid_simple_recovery=FALSE 时： 
#    为了初始化 gtid_executed，算法是： 从newest_binlog -> oldest_binlog 方向遍历读取，如果发现有Previous_gtids_log_event ， 那么就停止遍历 
#    为了初始化 gtid_purged，算法是：   从oldest_binlog -> newest_binlog 方向遍历读取, 如果发现有Previous_gtids_log_event（not empty）或者 #至少有一个Gtid_log_event的文件，那么就停止遍历 
#3. 当binlog_gtid_simple_recovery=TRUE 时： 
#    为了初始化 gtid_executed ， 算法是： 只需要读取newest_binlog 
#    为了初始化 gtid_purged， 算法是： 只需要读取oldest_binlog 
#4. 当设置binlog_gtid_simple_recovery=TRUE ， 如果MySQL版本低于5.7.7 ， 可能会有gitd计算出错的可能，具体参考官方文档详细描述 
log_timestamps=system                   # 主要是控制 error log、slow_log、genera log，等等记录日志的显示时间参数，但不会影响 general log 和 slow log 写到表 (mysql.general_log, mysql.slow_log) 中的显示时间 
#transaction_write_set_extraction=MURMUR32      # 基于WRITESET的并行复制方式 
show_compatibility_56=on                # mysql兼容性是否兼容mysql5.6，这是是开启兼容 

lower_case_table_names=1        # 是否区分大小写 说明 0：区分大小写，1：不区分大小写 
read_only=1                     # 普通是否可读， 0：关闭可读， 1：开启可读 
super_read_only=1               # 管理员（super）用户是否可读，超级可读 ，0：关闭可读， 1：开启可读 

#sync_binlog = 1　　　　　　　　　　　　　　 # 默认，sync_binlog=0，表示MySQL不控制binlog的刷新，如果sync_binlog>0，表示每sync_binlog次事务提交， 
# sync_binlog=1了，表示每次事务提交，MySQL都会把binlog刷下去，是最安全但是性能损耗最大的设置。sync_binlog=1，多个事务同时提交，同样很大的影响MySQL和IO性能。 
# MySQL DBA设置的sync_binlog并不是最安全的1，而是100或者是0。这样牺牲一定的一致性，可以获得更高的并发和性能。 
#gtid_mode = on                         # 是否开启开启 基于 gtid 的复制， 5.7之后才出现的新特性 
#enforce_gtid_consistency = 1           #  
#log_slave_updates 
#binlog_format = row  
#relay_log = relay.log 
#relay_log_recovery = 1 
#binlog_gtid_simple_recovery = 1 
#slave_skip_errors = ddl_exist_errors 

########innodb settings######## 
# 根据您的服务器IOPS能力适当调整,只有当你在频繁写操作的时候才有意义 
# 一般配普通SSD盘的话，可以调整到 10000 - 20000 
# 配置高端PCIe SSD卡的话，则可以调整的更高，比如 50000 - 80000  
<!-- 1(SRV_FORCE_IGNORE_CORRUPT):忽略检查到的corrupt页。 -->
<!-- 2(SRV_FORCE_NO_BACKGROUND):阻止主线程的运行，如主线程需要执行full purge操作，会导致crash。 -->
<!-- 3(SRV_FORCE_NO_TRX_UNDO):不执行事务回滚操作。 -->
<!-- 4(SRV_FORCE_NO_IBUF_MERGE):不执行插入缓冲的合并操作。 -->
<!-- 5(SRV_FORCE_NO_UNDO_LOG_SCAN):不查看重做日志，InnoDB存储引擎会将未提交的事务视为已提交。 -->
<!-- 6(SRV_FORCE_NO_LOG_REDO):不执行前滚的操作。 -->
innodb_force_recovery = 0
innodb_io_capacity = 4000               # 动态调整刷新脏页的数量，一般设置最大值的1/2 
innodb_io_capacity_max = 8000           #动态调整刷新脏页的最大数量 
innodb_buffer_pool_size = 200M          # 缓存池大小，默认128M，建议设置为总内存大小的，设置为物理内存的80% 
innodb_buffer_pool_instances = 8        # 可以开启多个内存缓冲池，把需要缓冲的数据hash到不同的缓冲池中，这样可以并行的内存读写。 
innodb_buffer_pool_load_at_startup = 1  # 默认为关闭OFF。如果开启该参数，启动MySQL服务时，MySQL将本地热数据加载到InnoDB缓冲池中。 
innodb_buffer_pool_dump_at_shutdown = 1 # 默认启用。指定在MySQL服务器关闭时是否记录在InnoDB缓冲池中缓存的页面，以便在下次重新启动时缩短预热过程。 
innodb_lru_scan_depth = 2000            # LRU列表中可用页的数量，默认值为1024。 
innodb_lock_wait_timeout = 5            # 事务锁超时时间  
#innodb_flush_method = O_DIRECT 

innodb_log_file_size = 200M             # mysql事务日志文件（ib_logfile0）的大小； 
innodb_log_files_in_group = 2           # 指定你有几个日志组。一般２-３个日值组。默认为两个。 
innodb_log_buffer_size = 16M            # 事务在内存中的缓冲大小。 

innodb_undo_logs = 128                  # InnoDB使用的回滚段个数，必须设置35个以上；，默认128 
innodb_undo_tablespaces = 3             # 是控制undo是否开启独立的表空间的参数，为0表示：undo使用系统表空间，即ibdata1，不为0表示：使用独立的表空间，一般名称为 undo001 undo002，存放地址的配置项为：innodb_undo_directory，默认配置为0，参数必须大于或等于2，即回收（收缩）一个undo log日志文件时，要保证另一个undo log是可用的。 
innodb_undo_log_truncate = 1            # 参数设置为1，即开启在线回收（收缩）undo log日志文件，支持动态设置。 
innodb_max_undo_log_size = 2G           # 每一个undo日志文件大小，默认1G 

innodb_flush_neighbors = 1              # 刷脏页的控制策略，参数就是用来控制这个行为的，值为 1 的时候会有上述的“连坐”机制，值为 0 时表示不找邻居，自己刷自己的。建议设置为0 
innodb_purge_threads = 4                # 负责回收已经使用并分配的undo页，可以指定多个innodb_purge_threads来进一步加快和提高undo回收速度。 
innodb_large_prefix = 1                 # 单索引限制，是否开启允许列索引最大达到3072，不开启只有767 
innodb_thread_concurrency = 64          # 来限制并发线程的数量 
innodb_print_all_deadlocks = 1          # 是否开启保存死锁日志，死锁日志存放到error_log配置的文件里面 
innodb_strict_mode = 1                  # InnoDB严格检查模式，尤其采用了页数据压缩功能后，最好是开启该功能。开启此功能后，当创建表（CREATE TABLE）、更改表（ALTER TABLE）和创建索引（CREATE INDEX）语句时，如果写法有错误，不会有警告信息，而是直接抛出错误，这样就可直接将问题扼杀在摇篮 
innodb_sort_buffer_size = 64M 
innodb_flush_log_at_trx_commit=1        # sync_binlog 两个参数是控制MySQL 磁盘写入策略以及数据安全性的关键参数，当两个参数都设置为1的时候写入性能最差，但安全性最高， 
# 设置为0，log buffer将每秒一次地写入log file中，并且log file的flush(刷到磁盘)操作同时进行.该模式下，在事务提交的时候，不会主动触发写入磁盘的操作。 
# 设置为1，每次事务提交时MySQL都会把log buffer的数据写入log file，并且flush(刷到磁盘)中去. 
# 设置为2，每次事务提交时MySQL都会把log buffer的数据写入log file.但是flush(刷到磁盘)操作并不会同时进行。该模式下,MySQL会每秒执行一次 flush(刷到磁盘)操作。 
innodb_autoextend_increment=64          # 默认 8M ,单位为M，配置表空间自动扩展，每次扩展多大M 
innodb_concurrency_tickets=5000         # 这个参数设置为一种tickets,默认是5000， 
innodb_old_blocks_time=1000             # 页读取到mid位置后，需要等待多久才会被加入到LRU列表的热端。默认1000ms 
innodb_open_files=65536                 # 限制Innodb能打开的表的数据。 
innodb_stats_on_metadata=0              # 是否自动更新统计信息,默认为0关闭， 
innodb_file_per_table=1                 # MySQL InnoDB引擎 默认会将所有的数据库InnoDB引擎的表数据存储在一个共享空间中：ibdata1，当增删数据库的时候，ibdata1文件不会自动收缩，单个数据库的备份也将成为问题。通常只能将数据使用mysqldump 导出，然后再导入解决这个问题。如果启用了innodb_file_per_talbe参数，需要注意的是每张表的表空间内存放的只是数据、索引和插入缓冲Bitmap页，其他数据如：回滚信息、插入缓冲索引页、系统事物信息、二次写缓冲（Double write buffer）等还是放在原来的共享表空间内。同时说明了一个问题：即使启用了innodb_file_per_table参数共享表空间还是会不断的增加其大小的。 
#innodb_checksum_algorithm=0            # 是否开启checksum算法 
innodb_data_file_path=ibdata1:200M;ibdata2:200M;ibdata3:200M:autoextend:max:5G      #可配置表空间相关参数。 
innodb_temp_data_file_path = ibtmp1:200M:autoextend:max:20G         # 可配置临时表空间相关参数。 
innodb_buffer_pool_dump_pct = 40        # 指定每个缓冲池最近使用的页面读取和转储的百分比，1-100，默认25 
innodb_page_cleaners = 4                # 多个页面清理线程刷脏页，用于指定页面清理线程的数量。其默认值1，维持了之前单个页面清理线程的配置 
innodb_purge_rseg_truncate_frequency = 128      # 指定purge操作被唤起多少次之后才释放rollback segments。当undo表空间里面的rollback segments被释放时，undo表空间才会被truncate。由此可见，该参数越小，undo表空间被尝试truncate的频率越高。 
binlog_gtid_simple_recovery=1           #这个变量用于在MySQL重启或启动的时候寻找GTIDs过程中，控制binlog 如何遍历的算法？ 
#2. 当binlog_gtid_simple_recovery=FALSE 时： 
#    为了初始化 gtid_executed，算法是： 从newest_binlog -> oldest_binlog 方向遍历读取，如果发现有Previous_gtids_log_event ， 那么就停止遍历 
#    为了初始化 gtid_purged，算法是：   从oldest_binlog -> newest_binlog 方向遍历读取, 如果发现有Previous_gtids_log_event（not empty）或者 #至少有一个Gtid_log_event的文件，那么就停止遍历 
#3. 当binlog_gtid_simple_recovery=TRUE 时： 
#    为了初始化 gtid_executed ， 算法是： 只需要读取newest_binlog 
#    为了初始化 gtid_purged， 算法是： 只需要读取oldest_binlog 
#4. 当设置binlog_gtid_simple_recovery=TRUE ， 如果MySQL版本低于5.7.7 ， 可能会有gitd计算出错的可能，具体参考官方文档详细描述 
log_timestamps=system                   # 主要是控制 error log、slow_log、genera log，等等记录日志的显示时间参数，但不会影响 general log 和 slow log 写到表 (mysql.general_log, mysql.slow_log) 中的显示时间 
#transaction_write_set_extraction=MURMUR32      # 基于WRITESET的并行复制方式 
show_compatibility_56=on                # mysql兼容性是否兼容mysql5.6，这是是开启兼容 
lower_case_table_names=1        # 是否区分大小写 说明 0：区分大小写，1：不区分大小写 
read_only=1                     # 普通是否可读， 0：关闭可读， 1：开启可读 
super_read_only=1               # 管理员（super）用户是否可读，超级可读 ，0：关闭可读， 1：开启可读 
```
