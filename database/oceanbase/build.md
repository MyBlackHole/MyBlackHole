# build

构建

- ubuntu 20.04 (官方推荐支持)
```shell
apt-get install git wget rpm rpm2cpio cpio make build-essential binutils m4

bash build.sh debug --init --make (或许失败)

<!-- 失败就 -->
cd build_debug

make observer
```

- podman 部署 (麻烦，需要 root)
```shell
# # deploy an instance of the largest size according to the current container
# docker run -p 2881:2881 --name obstandalone -d oceanbase/oceanbase-ce
#
# # deploy mini standalone instance
# docker run -p 2881:2881 --name obstandalone -e MINI_MODE=1 -d oceanbase/oceanbase-ce

# # centos7 源没有
# podman run --name dev -v /run/media/black/Data:/run/media/black/Data/ -it centos:centos7 bash

podman pull ubuntu:20.04
# --network=podman 加入 podman 网络(默认存在)
podman run --name dev --network=podman -v /run/media/black/Data:/run/media/black/Data/ -it ubuntu:20.04 bash
apt update
apt-get install git wget rpm rpm2cpio cpio make build-essential binutils m4
cd /run/media/black/Data/Documents/github/cpp/oceanbase
bash ./build.sh debug --init --make
```

- 在 centos7 构建 (官方推荐支持)
```shell
yum install git wget rpm* cpio make glibc-devel glibc-headers binutils m4 libtool libaio
mkdir -p /run/media/black/Data/Documents/github/cpp/oceanbase
sshfs -o nonempty root@10.5.13.27:/run/media/black/Data/Documents/github/cpp/oceanbase /run/media/black/Data/Documents/github/cpp/oceanbase
bash ./build.sh debug --init --make
bash build.sh debug --init --make
```

- 在 archlinux 构建
```shell
ld.lld: error: undefined symbol: pthread_mutex_consistent_np

paru -S rpmextract
./build.sh init
./build.sh debug --make -j4

用系统的libapr 0.7.4 库替换依赖包中的libapr 0.6.5库
ls -alh deps/3rd/usr/local/oceanbase/deps/devel/lib/libapr*
Permissions Size User  Date Modified Name
.rw-r--r--  448k black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libapr-1.a
.rwxr-xr-x  1.1k black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libapr-1.la
lrwxrwxrwx     - black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libapr-1.so -> libapr-1.so.0.6.5
lrwxrwxrwx     - black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libapr-1.so.0 -> libapr-1.so.0.6.5
.rwxr-xr-x  316k black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libapr-1.so.0.6.5
.rw-r--r--  344k black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libaprutil-1.a
.rwxr-xr-x  1.4k black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libaprutil-1.la
lrwxrwxrwx     - black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libaprutil-1.so -> libaprutil-1.so.0.6.1
lrwxrwxrwx     - black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libaprutil-1.so.0 -> libaprutil-1.so.0.6.1
.rwxr-xr-x  254k black  6 Sep  2022   deps/3rd/usr/local/oceanbase/deps/devel/lib/libaprutil-1.so.0.6.1

ls -alh /usr/lib/libapr*
Permissions Size User Date Modified Name
lrwxrwxrwx     - root 26 Oct  2023   /usr/lib/libapr-1.so -> libapr-1.so.0.7.4
lrwxrwxrwx     - root 26 Oct  2023   /usr/lib/libapr-1.so.0 -> libapr-1.so.0.7.4
.rwxr-xr-x  248k root 26 Oct  2023   /usr/lib/libapr-1.so.0.7.4
lrwxrwxrwx     - root 12 Feb  2023   /usr/lib/libaprutil-1.so -> libaprutil-1.so.0.6.3
lrwxrwxrwx     - root 12 Feb  2023   /usr/lib/libaprutil-1.so.0 -> libaprutil-1.so.0.6.3
.rwxr-xr-x  187k root 12 Feb  2023   /usr/lib/libaprutil-1.so.0.6.3

cp -rf /usr/lib/libapr* deps/3rd/usr/local/oceanbase/deps/devel/lib/

# 生成 single.yaml
./tools/deploy/obd.sh prepare
# 启动 observer 单实例
./tools/deploy/obd.sh deploy -c ./tools/deploy/single.yaml
# 销毁单实例 observer
./tools/deploy/obd.sh destroy --rm -n single
```


## 编译 (3.x)
```shell
bash build.sh debug --init --make
<!-- bash build.sh release --init --make -->

./tools/deploy/obd.sh prepare -p /tmp/obtest
./tools/deploy/obd.sh deploy -c ./tools/deploy/single.yaml

lvim observer.include.yaml
runtime_dependencies:
  - src_path: plugin_dir
    target_path: plugin_dir
  - src_path: admin
    target_path: admin
  - src_path: lib
    target_path: lib
  - src_path: tools
    target_path: tools
  - src_path: etc
    target_path: etc
  - src_path: wallet
    target_path: wallet
env:
  ASAN_OPTIONS: 'abort_on_error=1:disable_coredump=0:unmap_shadow_on_exit=1:log_path=./log/asan.log'
  ASAN_SYMBOLIZER_PATH: ./tools/llvm-symbolizer
config:
  cluster_id: 1
  major_freeze_duty_time: 'Disable'
  # datafile_disk_percentage: '50'
  datafile_size: '4G'
  # 最低要求 4G
  memory_limit: '4G'
  system_memory: '2G'
  cpu_count: '8'
  stack_size: '512K'
  cache_wash_threshold: '1G'
  __min_full_resource_pool_memory: "268435456"
  workers_per_cpu_quota: '10'
  schema_history_expire_time: '1d'
  net_thread_count: '2'
  minor_freeze_times: '10'
  enable_separate_sys_clog: 'FALSE'
  enable_merge_by_turn: 'FALSE'
  syslog_io_bandwidth_limit: '1G'
  enable_async_syslog: 'FALSE'



<!-- mycli -uroot -h127.0.0.1 -P10000 (显示有问题)-->
mysql -uroot -h127.0.0.1 -P10000
<!-- ./deps/3rd/u01/obclient/bin/obclient -h127.0.0.1 -P10000 -uroot -Doceanbase -A -->


MySQL [(none)]> SELECT * FROM oceanbase.gv$tenant;
+-----------+-------------+-----------+--------------+----------------+---------------+-----------+---------------+
| tenant_id | tenant_name | zone_list | primary_zone | collation_type | info          | read_only | locality      |
+-----------+-------------+-----------+--------------+----------------+---------------+-----------+---------------+
|         1 | sys         | zone1     | zone1        |              0 | system tenant |         0 | FULL{1}@zone1 |
+-----------+-------------+-----------+--------------+----------------+---------------+-----------+---------------+
1 row in set (0.001 sec)

MySQL [(none)]> SHOW PARAMETERS LIKE 'backup_dest'\G
*************************** 1. row ***************************
      zone: zone1
  svr_type: observer
    svr_ip: 127.0.0.1
  svr_port: 10001
      name: backup_dest
 data_type: NULL
     value:
      info: backup dest
   section: OBSERVER
     scope: CLUSTER
    source: DEFAULT
edit_level: DYNAMIC_EFFECTIVE
1 row in set (0.007 sec)


<!-- 配置归档目录 -->
ALTER SYSTEM SET backup_dest='file:///obtest/archive';

MySQL [(none)]> SHOW PARAMETERS LIKE 'backup_dest'\G
*************************** 1. row ***************************
      zone: zone1
  svr_type: observer
    svr_ip: 127.0.0.1
  svr_port: 10001
      name: backup_dest
 data_type: NULL
     value: file:///obtest/archive
      info: backup dest
   section: OBSERVER
     scope: CLUSTER
    source: DEFAULT
edit_level: DYNAMIC_EFFECTIVE
1 row in set (0.007 sec)


alter system archivelog;


MySQL [oceanbase]> SELECT * FROM CDB_OB_BACKUP_ARCHIVELOG;
+-------------+-------------------+-----------+--------+----------------+-----------------+----------------------------+----------------------------+-------------+--------------+-------------------+---------------------+----------------------+
| INCARNATION | LOG_ARCHIVE_ROUND | TENANT_ID | STATUS | START_PIECE_ID | BACKUP_PIECE_ID | MIN_FIRST_TIME             | MAX_NEXT_TIME              | INPUT_BYTES | OUTPUT_BYTES | COMPRESSION_RATIO | INPUT_BYTES_DISPLAY | OUTPUT_BYTES_DISPLAY |
+-------------+-------------------+-----------+--------+----------------+-----------------+----------------------------+----------------------------+-------------+--------------+-------------------+---------------------+----------------------+
|           1 |                 1 |         1 | DOING  |              0 |               0 | 2024-04-13 08:53:47.460682 | 2024-04-13 09:01:06.684902 |           0 |            0 |              NULL | 0.00MB              | 0.00MB               |
+-------------+-------------------+-----------+--------+----------------+-----------------+----------------------------+----------------------------+-------------+--------------+-------------------+---------------------+----------------------+
1 row in set (0.018 sec)


MySQL [oceanbase]> SELECT * FROM oceanbase.__all_resource_pool;
+----------------------------+----------------------------+------------------+----------+------------+----------------+-----------+-----------+--------------+--------------------+
| gmt_create                 | gmt_modified               | resource_pool_id | name     | unit_count | unit_config_id | zone_list | tenant_id | replica_type | is_tenant_sys_pool |
+----------------------------+----------------------------+------------------+----------+------------+----------------+-----------+-----------+--------------+--------------------+
| 2024-04-13 08:40:54.961873 | 2024-04-13 08:40:54.985238 |                1 | sys_pool |          1 |              1 | zone1     |         1 |            0 |                  0 |
+----------------------------+----------------------------+------------------+----------+------------+----------------+-----------+-----------+--------------+--------------------+
1 row in set (0.006 sec)


MySQL [oceanbase]> SELECT * FROM oceanbase.__all_unit_config;
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| gmt_create                 | gmt_modified               | unit_config_id | name            | max_cpu | min_cpu | max_memory | min_memory | max_iops | min_iops | max_disk_size | max_session_num     |
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| 2024-04-13 08:40:54.951374 | 2024-04-13 08:40:54.951374 |              1 | sys_unit_config |       5 |     2.5 |  644245094 |  536870912 |    10000 |     5000 |    4294967296 | 9223372036854775807 |
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
1 row in set (0.004 sec)


MySQL [oceanbase]> CREATE RESOURCE UNIT unit1 MAX_CPU 1, MAX_MEMORY '1G', MAX_IOPS 128,MAX_DISK_SIZE '1G', MAX_SESSION_NUM 64, MIN_CPU=1, MIN_MEMORY= '1G', MIN_IOPS=128;
Query OK, 0 rows affected (0.025 sec)

MySQL [oceanbase]> SELECT * FROM oceanbase.__all_unit_config;
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| gmt_create                 | gmt_modified               | unit_config_id | name            | max_cpu | min_cpu | max_memory | min_memory | max_iops | min_iops | max_disk_size | max_session_num     |
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| 2024-04-13 08:40:54.951374 | 2024-04-13 08:40:54.951374 |              1 | sys_unit_config |       5 |     2.5 |  644245094 |  536870912 |    10000 |     5000 |    4294967296 | 9223372036854775807 |
| 2024-04-13 09:11:16.260320 | 2024-04-13 09:11:16.260320 |           1001 | unit1           |       1 |       1 | 1073741824 | 1073741824 |      128 |      128 |    1073741824 |                  64 |
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
2 rows in set (0.001 sec)


MySQL [oceanbase]> CREATE RESOURCE POOL pool1 UNIT='unit1',UNIT_NUM=1,ZONE_LIST=('zone1');
Query OK, 0 rows affected (0.031 sec)

MySQL [oceanbase]> SELECT * FROM oceanbase.__all_resource_pool;
+----------------------------+----------------------------+------------------+----------+------------+----------------+-----------+-----------+--------------+--------------------+
| gmt_create                 | gmt_modified               | resource_pool_id | name     | unit_count | unit_config_id | zone_list | tenant_id | replica_type | is_tenant_sys_pool |
+----------------------------+----------------------------+------------------+----------+------------+----------------+-----------+-----------+--------------+--------------------+
| 2024-04-13 08:40:54.961873 | 2024-04-13 08:40:54.985238 |                1 | sys_pool |          1 |              1 | zone1     |         1 |            0 |                  0 |
| 2024-04-13 09:18:00.894799 | 2024-04-13 09:18:00.894799 |             1004 | pool1    |          1 |           1002 | zone1     |        -1 |            0 |                  0 |
+----------------------------+----------------------------+------------------+----------+------------+----------------+-----------+-----------+--------------+--------------------+
2 rows in set (0.001 sec)


MySQL [oceanbase]> CREATE TENANT IF NOT EXISTS tenant1 CHARSET='utf8mb4',  RESOURCE_POOL_LIST=('pool1');
Query OK, 0 rows affected (2.282 sec)

MySQL [oceanbase]> SELECT * FROM oceanbase.gv$tenant;
+-----------+-------------+-----------+--------------+----------------+---------------+-----------+---------------+
| tenant_id | tenant_name | zone_list | primary_zone | collation_type | info          | read_only | locality      |
+-----------+-------------+-----------+--------------+----------------+---------------+-----------+---------------+
|         1 | sys         | zone1     | zone1        |              0 | system tenant |         0 | FULL{1}@zone1 |
|      1001 | tenant1     | zone1     | RANDOM       |              0 |               |         0 | FULL{1}@zone1 |
+-----------+-------------+-----------+--------------+----------------+---------------+-----------+---------------+
2 rows in set (0.008 sec)

./tools/deploy/obd.sh stop -c ./tools/deploy/single.yaml

./tools/deploy/obd.sh start -c ./tools/deploy/single.yaml

MySQL [(none)]> alter system noarchivelog;
Query OK, 0 rows affected (0.095 sec)


MySQL [oceanbase]> SELECT * FROM CDB_OB_BACKUP_ARCHIVELOG;
+-------------+-------------------+-----------+--------+----------------+-----------------+----------------------------+----------------------------+-------------+--------------+-------------------+---------------------+----------------------+
| INCARNATION | LOG_ARCHIVE_ROUND | TENANT_ID | STATUS | START_PIECE_ID | BACKUP_PIECE_ID | MIN_FIRST_TIME             | MAX_NEXT_TIME              | INPUT_BYTES | OUTPUT_BYTES | COMPRESSION_RATIO | INPUT_BYTES_DISPLAY | OUTPUT_BYTES_DISPLAY |
+-------------+-------------------+-----------+--------+----------------+-----------------+----------------------------+----------------------------+-------------+--------------+-------------------+---------------------+----------------------+
|           1 |                 1 |         1 | STOP   |              0 |               0 | 2024-04-13 08:53:47.460682 | 2024-04-13 16:33:43.624022 |           0 |            0 |              NULL | 0.00MB              | 0.00MB               |
+-------------+-------------------+-----------+--------+----------------+-----------------+----------------------------+----------------------------+-------------+--------------+-------------------+---------------------+----------------------+
1 row in set (0.036 sec)



MySQL [oceanbase]> alter system archivelog;
Query OK, 0 rows affected (0.114 sec)

MySQL [oceanbase]> SELECT * FROM CDB_OB_BACKUP_ARCHIVELOG;
+-------------+-------------------+-----------+-----------+----------------+-----------------+----------------------------+---------------+-------------+--------------+-------------------+---------------------+----------------------+
| INCARNATION | LOG_ARCHIVE_ROUND | TENANT_ID | STATUS    | START_PIECE_ID | BACKUP_PIECE_ID | MIN_FIRST_TIME             | MAX_NEXT_TIME | INPUT_BYTES | OUTPUT_BYTES | COMPRESSION_RATIO | INPUT_BYTES_DISPLAY | OUTPUT_BYTES_DISPLAY |
+-------------+-------------------+-----------+-----------+----------------+-----------------+----------------------------+---------------+-------------+--------------+-------------------+---------------------+----------------------+
|           1 |                 2 |         1 | BEGINNING |              0 |               0 | 2024-04-13 16:35:56.854341 |               |           0 |            0 |              NULL | 0.00MB              | 0.00MB               |
|           1 |                 2 |      1001 | BEGINNING |              0 |               0 | 2024-04-13 09:19:46.345823 |               |           0 |            0 |              NULL | 0.00MB              | 0.00MB               |
+-------------+-------------------+-----------+-----------+----------------+-----------------+----------------------------+---------------+-------------+--------------+-------------------+---------------------+----------------------+
2 rows in set (0.001 sec)

<!-- 全量 -->
MySQL [oceanbase]> alter system backup database;
Query OK, 0 rows affected (0.404 sec)


MySQL [oceanbase]> SELECT * FROM CDB_OB_BACKUP_SET_DETAILS;
+-------------+-----------+--------+---------+-------------+-----------------+----------------------------+----------------------------+------------------+------+------------+-------------+------------+--------------+-------------------+-------------------+----------------------+---------------------------+--------------------+-----------+
| INCARNATION | TENANT_ID | BS_KEY | COPY_ID | BACKUP_TYPE | ENCRYPTION_MODE | START_TIME                 | COMPLETION_TIME            | ELAPSED_SECONDES | KEEP | KEEP_UNTIL | DEVICE_TYPE | COMPRESSED | OUTPUT_BYTES | OUTPUT_RATE_BYTES | COMPRESSION_RATIO | OUTPUT_BYTES_DISPLAY | OUTPUT_RATE_BYTES_DISPLAY | TIME_TAKEN_DISPLAY | STATUS    |
+-------------+-----------+--------+---------+-------------+-----------------+----------------------------+----------------------------+------------------+------+------------+-------------+------------+--------------+-------------------+-------------------+----------------------+---------------------------+--------------------+-----------+
|           1 |         1 |      1 | 0       | D           | NONE            | 2024-04-13 16:37:32.780408 | 2024-04-13 16:38:07.432236 |               35 | NO   |            | FILE        | NO         |      1710280 |        49356.1263 |              0.05 | 1.63MB               | 0.05MB/S                  | 00:00:34.651828    | COMPLETED |
|           1 |      1001 |      1 | 0       | D           | NONE            | 2024-04-13 16:37:32.780408 | 2024-04-13 16:38:06.873715 |               34 | NO   |            | FILE        | NO         |      1710280 |        50164.6848 |              0.05 | 1.63MB               | 0.05MB/S                  | 00:00:34.093307    | COMPLETED |
+-------------+-----------+--------+---------+-------------+-----------------+----------------------------+----------------------------+------------------+------+------------+-------------+------------+--------------+-------------------+-------------------+----------------------+---------------------------+--------------------+-----------+
2 rows in set (0.019 sec)

<!-- 增量 -->
ALTER SYSTEM BACKUP INCREMENTAL DATABASE;


MySQL [oceanbase]> SELECT * FROM CDB_OB_BACKUP_PROGRESS;
+-------------+--------+-------------+-----------+-----------------+-------------------+------------------------+--------------------------+-------------+--------------+----------------------------+----------------------------+---------+
| INCARNATION | BS_KEY | BACKUP_TYPE | TENANT_ID | PARTITION_COUNT | MACRO_BLOCK_COUNT | FINISH_PARTITION_COUNT | FINISH_MACRO_BLOCK_COUNT | INPUT_BYTES | OUTPUT_BYTES | START_TIME                 | COMPLETION_TIME            | STATUS  |
+-------------+--------+-------------+-----------+-----------------+-------------------+------------------------+--------------------------+-------------+--------------+----------------------------+----------------------------+---------+
|           1 |      2 | I           |         1 |               0 |                 0 |                      0 |                        0 |           0 |            0 | 2024-04-13 16:43:28.807222 | 2024-04-13 16:43:29.123073 | RUNNING |
|           1 |      2 | I           |      1001 |             122 |                 0 |                      0 |                        0 |           0 |            0 | 2024-04-13 16:43:28.807222 | 2024-04-13 16:43:29.061447 | RUNNING |
+-------------+--------+-------------+-----------+-----------------+-------------------+------------------------+--------------------------+-------------+--------------+----------------------------+----------------------------+---------+
2 rows in set (0.006 sec)


./build_debug/tools/ob_admin/ob_admin dump_backup -d file:///obtest/archive/obcluster/1/incarnation_1/1001/data/tenant_data_backup_info
------------------------------{Common Header}------------------------------
|                     data_type|16716
|                header_version|1
|               compressor_type|0
|                     data_type|16716
|                  data_version|0
|                 header_length|48
|               header_checksum|0
|                  data_length_|0
|                  data_zlength|0
|                 data_checksum|0
|                  align_length|0
--------------------------------------------------------------------------------
------------------------------{BackupSet[1]}------------------------------
|                             1|{full_backup_set_id:1, inc_backup_set_id:1, backup_data_version:1, backup_snapshot_version:1712997452780408, backup_schema_version:1712971188224752, frozen_snapshot_version:1, frozen_schema_version:0, prev_full_backup_set_id:0, prev_inc_backup_set_id:0, prev_backup_data_version:0, compatible:4, cluster_version:12884967429, backup_type:1, status:0, encryption_mode:0, passwd:"", is_mark_deleted:false, date:20240413}
------------------------------{BackupSet[1]}------------------------------
|                             2|{full_backup_set_id:1, inc_backup_set_id:2, backup_data_version:1, backup_snapshot_version:1712997808807222, backup_schema_version:1712971188224752, frozen_snapshot_version:1, frozen_schema_version:0, prev_full_backup_set_id:1, prev_inc_backup_set_id:1, prev_backup_data_version:1, compatible:4, cluster_version:12884967429, backup_type:2, status:1, encryption_mode:0, passwd:"", is_mark_deleted:false, date:20240413}
--------------------------------------------------------------------------------


./build_debug/tools/ob_admin/ob_admin dump_backup -d file:///obtest/archive/obcluster/1/incarnation_1/1001/data/tenant_backup_set_file_info
------------------------------{Common Header}------------------------------
|                     data_type|16716
|                header_version|1
|               compressor_type|0
|                     data_type|16716
|                  data_version|0
|                 header_length|48
|               header_checksum|0
|                  data_length_|0
|                  data_zlength|0
|                 data_checksum|0
|                  align_length|0
--------------------------------------------------------------------------------
|                             1|{tenant_id:1001, backup_set_id:1, incarnation:1, copy_id:0, snapshot_version:1712997452780408, prev_full_backup_set_id:0, prev_inc_backup_set_id:0, prev_backup_data_version:0, pg_count:122, macro_block_count:18, finish_pg_count:122, finish_macro_block_count:18, input_bytes:37843926, output_bytes:1710280, start_time:1712997452780408, end_time:1712997486873715, compatible:4, cluster_version:12884967429, backup_type:{type:1}, status:1, result:0, cluster_id:1, backup_dest:"file:///obtest/archive", backup_data_version:1, backup_schema_version:1712971188224752, partition_count:122, finish_partition_count:122, encryption_mode:0, passwd:"", file_status:0, start_replay_log_ts:1712997143592497, date:20240413}
|                             2|{tenant_id:1001, backup_set_id:2, incarnation:1, copy_id:0, snapshot_version:1712997808807222, prev_full_backup_set_id:1, prev_inc_backup_set_id:1, prev_backup_data_version:1, pg_count:0, macro_block_count:0, finish_pg_count:0, finish_macro_block_count:0, input_bytes:0, output_bytes:0, start_time:0, end_time:0, compatible:4, cluster_version:12884967429, backup_type:{type:2}, status:0, result:0, cluster_id:1, backup_dest:"file:///obtest/archive", backup_data_version:1, backup_schema_version:1712971188224752, partition_count:0, finish_partition_count:0, encryption_mode:0, passwd:"", file_status:1, start_replay_log_ts:0, date:20240413}
--------------------------------------------------------------------------------

./build_debug/tools/ob_admin/ob_admin dump_backup -d file:///obtest/archive/obcluster/1/incarnation_1/1001/clog/backup_piece_info
------------------------------{Common Header}------------------------------
|                     data_type|16716
|                header_version|1
|               compressor_type|0
|                     data_type|16716
|                  data_version|0
|                 header_length|48
|               header_checksum|0
|                  data_length_|0
|                  data_zlength|0
|                 data_checksum|0
|                  align_length|0
--------------------------------------------------------------------------------
|                             0|{key:{incarnation:1, tenant_id:1001, round_id:1, backup_piece_id:0, copy_id:0}, create_date:20240413, status:2, file_status:0, start_ts:1712971186345823, checkpoint_ts:1712997223624022, max_ts:1712997303927747, backup_dest:"file:///obtest/archive", status_str:"FROZEN", file_status_str:"AVAILABLE", compatible:1, start_piece_id:0}
|                             0|{key:{incarnation:1, tenant_id:1001, round_id:2, backup_piece_id:0, copy_id:0}, create_date:20240413, status:0, file_status:0, start_ts:1712997356854341, checkpoint_ts:0, max_ts:9223372036854775807, backup_dest:"file:///obtest/archive", status_str:"ACTIVE", file_status_str:"AVAILABLE", compatible:1, start_piece_id:0}
--------------------------------------------------------------------------------


./build_debug/tools/ob_admin/ob_admin dump_backup -d file:///obtest/archive/obcluster/1/incarnation_1/1001/clog/tenant_clog_backup_info
------------------------------{Common Header}------------------------------
|                     data_type|16716
|                header_version|1
|               compressor_type|0
|                     data_type|16716
|                  data_version|0
|                 header_length|48
|               header_checksum|0
|                  data_length_|0
|                  data_zlength|0
|                 data_checksum|0
|                  align_length|0
--------------------------------------------------------------------------------
|                          1001|{tenant_id:1001, copy_id:0, start_ts:1712971186345823, checkpoint_ts:1712997223624022, status:1, incarnation:1, round:1, status_str:"STOP", is_mark_deleted:false, is_mount_file_created:true, compatible:1, backup_piece_id:0, start_piece_id:0}
|                          1001|{tenant_id:1001, copy_id:0, start_ts:1712997356854341, checkpoint_ts:1712998552266934, status:3, incarnation:1, round:2, status_str:"DOING", is_mark_deleted:false, is_mount_file_created:true, compatible:1, backup_piece_id:0, start_piece_id:0}
--------------------------------------------------------------------------------



./tools/deploy/obd.sh destroy --rm -n single
```
