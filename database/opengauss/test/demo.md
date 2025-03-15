# demo

```shell


show data_directory;
     data_directory
-------------------------
/var/lib/opengauss/data
(1 row)


mkdir -p /home/opengauss/backup_1/wal

gs_probackup add-instance -B /home/opengauss/backup_1 -D /var/lib/opengauss/data --instance gs_master_backup_1

gs_probackup backup -B /home/opengauss/backup_1 --instance gs_master_backup_1 -b full -D /var/lib/opengauss/data --progress \
    --log-directory=/home/opengauss/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=full_20230516.log \
    --note='full backup test 1'

gs_probackup show -B /home/opengauss/backup_1/
BACKUP INSTANCE 'gs_master_backup_1'
=================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode  WAL Mode  TLI  Time   Data   WAL  Zratio  Start LSN  Stop LSN   Type  Status
=================================================================================================================================================
 gs_master_backup_1  9.2      SRQZOC  2025-02-16 07:24:14+08  FULL  STREAM    1/0    7s  596MB  16MB    1.06  0/30000A8  0/3000268  FILE  OK



CREATE DATABASE tpcds;

tpcds=# select oid,datname from pg_database where datname = 'tpcds';
  oid  | datname
-------+---------
 16389 | tpcds
(1 row)

\c tpcds;
CREATE TABLE warehouse_t1 (
    W_WAREHOUSE_SK            INTEGER               NOT NULL,
    W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
    W_WAREHOUSE_NAME          VARCHAR(20)                   ,
    W_WAREHOUSE_SQ_FT         INTEGER                       ,
    W_STREET_NUMBER           CHAR(10)                      ,
    W_STREET_NAME             VARCHAR(60)                   ,
    W_STREET_TYPE             CHAR(15)                      ,
    W_SUITE_NUMBER            CHAR(10)                      ,
    W_CITY                    VARCHAR(60)                   ,
    W_COUNTY                  VARCHAR(30)                   ,
    W_STATE                   CHAR(2)                       ,
    W_ZIP                     CHAR(10)                      ,
    W_COUNTRY                 VARCHAR(20)                   ,
    W_GMT_OFFSET              DECIMAL(5,2)
);

select * from pg_class where relname='warehouse_t1';
select oid,relfilenode,relname from pg_class where relname='warehouse_t1';
  oid  |   relname
-------+--------------
 16390 | warehouse_t1
(1 row)

select * from pg_class where relname='warehouse_t1';

INSERT INTO warehouse_t1 VALUES (1, 'WH10000001', 'Warehouse 1', 100000, '123', 'Apt 1', 'Residential', '123', 'Anytown', 'Anytown', 'CA', '90210', -8.00);

ls -alh /var/lib/opengauss/data/base/16389/16390
-rw------- 1 opengauss opengauss 8.0K Feb 16 08:32 /var/lib/opengauss/data/base/16389/16390

ALTER TABLE warehouse_t1 ADD COLUMN insert_time TIMESTAMP WITH TIME ZONE DEFAULT NOW();


insert_data.sh
#!/bin/bash

# 配置数据库连接（安全提示：建议使用配置文件或环境变量）
HOST="localhost"
PORT="5432"
DBNAME="tpcds"
USER="your_user"
PASSWORD="your_password"

# 生成随机数据的函数
generate_data() {
    echo $((RANDOM%1000))                  # W_WAREHOUSE_SK
    echo "WH$(printf '%04d' $1)"           # W_WAREHOUSE_ID
    echo "Warehouse_$((RANDOM%100))"       # W_WAREHOUSE_NAME
    echo $((RANDOM*100))                   # W_WAREHOUSE_SQ_FT
    echo $((RANDOM%9999))                  # W_STREET_NUMBER
    echo "Street_$((RANDOM%100))"          # W_STREET_NAME
    echo "Type_$((RANDOM%10))"             # W_STREET_TYPE
    echo "Suite_$((RANDOM%100))"           # W_SUITE_NUMBER
    echo "City_$((RANDOM%100))"            # W_CITY
    echo "County_$((RANDOM%100))"          # W_COUNTY
    echo $(tr -dc A-Z </dev/urandom | head -c2)  # W_STATE
    echo "ZIP_$((RANDOM%10000))"           # W_ZIP
    echo "Country_$((RANDOM%100))"         # W_COUNTRY
    echo $(awk 'BEGIN{printf "%.2f", (rand()*24-12)}')  # W_GMT_OFFSET
}

# 插入函数（带重试机制）
execute_insert() {
    for attempt in {1..3}; do
        if gsql -d $DBNAME -c "$1"; then
            return 0
        else
            echo "[WARN] 插入失败，重试 $attempt/3..."
            sleep 1
        fi
    done
    echo "[ERROR] 插入失败，终止脚本"
    exit 1
}

# 主循环
count=1
while true; do
    start_time=$(date +%s.%N)

    # 生成数据
    read -r -d $'\n' -a data <<< "$(generate_data $count)"

    # 构造SQL
    SQL="INSERT INTO warehouse_t1 VALUES (
        ${data[0]},   '${data[1]}',  '${data[2]}',
        ${data[3]},   '${data[4]}',  '${data[5]}',
        '${data[6]}', '${data[7]}',  '${data[8]}',
        '${data[9]}', '${data[10]}', '${data[11]}',
        '${data[12]}', ${data[13]}, NOW()
    )"

    echo "$SQL"

    # 执行插入
    execute_insert "$SQL"
    echo "[INFO] 已插入记录 $count | 时间: $(date '+%T.%3N')"

    # 精确时间控制
    end_time=$(date +%s.%N)
    sleep_time=$(echo "1.000 - ($end_time - $start_time)" | bc)
    [ $(echo "$sleep_time > 0" | bc) -eq 1 ] && sleep $sleep_time

    ((count++))
done

chmod +x insert_data.sh
./insert_data.sh

/usr/local/opengauss/bin

DELETE FROM warehouse_t1;
ls -alh /var/lib/opengauss/data/base/16389/16390
-rw------- 1 opengauss opengauss 192K Feb 16 09:39 /var/lib/opengauss/data/base/16389/16390

ls -alh /var/lib/opengauss/data/base/16389/16390
-rw------- 1 opengauss opengauss 0 Feb 16 09:39 /var/lib/opengauss/data/base/16389/16390

VACUUM FULL warehouse_t1;


tpcds=# select oid,relfilenode,relname from pg_class where relname='warehouse_t1';
  oid  | relfilenode |   relname
-------+-------------+--------------
 16390 |       16394 | warehouse_t1
(1 row)


tpcds=# SELECT pg_cbm_tracked_location();
 pg_cbm_tracked_location
-------------------------
 00000000/04129A80
(1 row)



./gs_probackup backup -B /home/opengauss/backup_1 --instance gs_master_backup_1 -b PTRACK -D /var/lib/opengauss/data --progress \
    --log-directory=/home/opengauss/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=incr2.log \
    --note='incremental backup test 2'

SELECT pg_cbm_tracked_location();
 pg_cbm_tracked_location
-------------------------
 00000000/06000028
(1 row)



./gs_probackup show -B /home/opengauss/backup_1/
BACKUP INSTANCE 'gs_master_backup_1'
===================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode    WAL Mode  TLI  Time   Data   WAL  Zratio  Start LSN  Stop LSN   Type  Status
===================================================================================================================================================
 gs_master_backup_1  9.2      SRR74G  2025-02-16 10:05:05+08  PTRACK  STREAM    1/1    6s  270MB  16MB    1.00  0/5000028  0/50001E8  FILE  OK
 gs_master_backup_1  9.2      SRQZOC  2025-02-16 07:24:14+08  FULL    STREAM    1/0    7s  596MB  16MB    1.06  0/30000A8  0/3000268  FILE  OK

## -i 指定备份文件ID
gs_probackup restore -B /home/opengauss/backup_1/ --instance=gs_master_backup_1 -D /home/opengauss/restore_data/ -i SRR74G --progress -j 4


export GAUSSHOME=/usr/local/opengauss
export PATH=$GAUSSHOME/bin:$PATH
export LD_LIBRARY_PATH=$GAUSSHOME/lib:$LD_LIBRARY_PATH

rm /home/opengauss/restore_data/postmaster.pid

vim /home/opengauss/restore_data/postgresql.conf
listen_addresses = '*'
port = 15432

gs_ctl start -D /home/opengauss/restore_data/ -w -t 3600
gsql -p 15432
opengauss=# \l
                               List of databases
   Name    |   Owner   | Encoding  | Collate | Ctype |    Access privileges
-----------+-----------+-----------+---------+-------+-------------------------
 opengauss | opengauss | SQL_ASCII | C       | C     |
 postgres  | opengauss | SQL_ASCII | C       | C     |
 template0 | opengauss | SQL_ASCII | C       | C     | =c/opengauss           +
           |           |           |         |       | opengauss=CTc/opengauss
 template1 | opengauss | SQL_ASCII | C       | C     | =c/opengauss           +
           |           |           |         |       | opengauss=CTc/opengauss
 tpcds     | opengauss | SQL_ASCII | C       | C     |
(5 rows)


./gs_ctl stop -D /home/opengauss/restore_data/ -w -t 3600
[2025-02-16 16:43:37.995][243768][][gs_ctl]: gs_ctl stopped ,datadir is /home/opengauss/restore_data
waiting for server to shut down....ls: cannot read symbolic link '/proc/243335/cwd': No such file or directory
 done
server stopped

update warehouse_t1 set W_WAREHOUSE_SK = 1 where W_WAREHOUSE_ID = '1111';
update warehouse_t1 set W_WAREHOUSE_SK = 2 where W_WAREHOUSE_ID = '2222';

gdb --args gs_probackup backup -B /home/opengauss/backup_1 --instance gs_master_backup_1 -b PTRACK -D /var/lib/opengauss/data --progress \
    --log-directory=/home/opengauss/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=incr3.log \
    --note='incremental backup test 3'
backup_files

\c tpcds;
DELETE FROM warehouse_t1;
VACUUM FULL warehouse_t1;
ls -alh /var/lib/opengauss/data/base/16389/16394

gs_probackup show -B /home/opengauss/backup_1/
BACKUP INSTANCE 'gs_master_backup_1'
=====================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode    WAL Mode  TLI    Time   Data   WAL  Zratio  Start LSN  Stop LSN   Type  Status
=====================================================================================================================================================
 gs_master_backup_1  9.2      SRRRCF  2025-02-16 17:24:01+08  PTRACK  STREAM    1/1  2m:15s  264MB  16MB    1.00  0/B000028  0/B236C20  FILE  OK
 gs_master_backup_1  9.2      SRRR9T  2025-02-16 17:20:18+08  PTRACK  STREAM    1/1      6s  282MB  16MB    0.94  0/9000028  0/9000318  FILE  OK
 gs_master_backup_1  9.2      SRRQ1G  2025-02-16 16:53:41+08  PTRACK  STREAM    1/1      6s  537MB  16MB    1.00  0/70000A8  0/7000268  FILE  OK
 gs_master_backup_1  9.2      SRR74G  2025-02-16 10:05:05+08  PTRACK  STREAM    1/1      6s  270MB  16MB    1.00  0/5000028  0/50001E8  FILE  OK
 gs_master_backup_1  9.2      SRQZOC  2025-02-16 07:24:14+08  FULL    STREAM    1/0      7s  596MB  16MB    1.06  0/30000A8  0/3000268  FILE  OK


vim /home/opengauss/restore_data/postgresql.conf
listen_addresses = '*'
port = 15432
gs_probackup restore -B /home/opengauss/backup_1/ --instance=gs_master_backup_1 -D /home/opengauss/restore_data_test/ -i SRRRCF --progress -j 4
gs_ctl start -D /home/opengauss/restore_data_test/ -w -t 3600
gs_ctl stop -D /home/opengauss/restore_data_test/ -w -t 3600


drop table warehouse_t1;

CREATE TABLE warehouse_t1 (
    W_WAREHOUSE_SK            INTEGER               NOT NULL,
    W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
    W_WAREHOUSE_NAME          VARCHAR(20)                   ,
    W_WAREHOUSE_SQ_FT         INTEGER                       ,
    W_STREET_NUMBER           CHAR(10)                      ,
    W_STREET_NAME             VARCHAR(60)                   ,
    W_STREET_TYPE             CHAR(15)                      ,
    W_SUITE_NUMBER            CHAR(10)                      ,
    W_CITY                    VARCHAR(60)                   ,
    W_COUNTY                  VARCHAR(30)                   ,
    W_STATE                   CHAR(2)                       ,
    W_ZIP                     CHAR(10)                      ,
    W_COUNTRY                 VARCHAR(20)                   ,
    W_GMT_OFFSET              DECIMAL(5,2)
);
ALTER TABLE warehouse_t1 ADD COLUMN insert_time TIMESTAMP WITH TIME ZONE DEFAULT NOW();

tpcds=# select oid,relfilenode,relname from pg_class where relname='warehouse_t1';
  oid  | relfilenode |   relname
-------+-------------+--------------
 16400 |       16400 | warehouse_t1
(1 row)

ls -alh /var/lib/opengauss/data/base/16389/16400
-rw------- 1 opengauss opengauss 0 Feb 16 17:52 /var/lib/opengauss/data/base/16389/16400

./update_data.sh

gs_probackup backup -B /home/opengauss/backup_1 --instance gs_master_backup_1 -b full -D /var/lib/opengauss/data --progress \
    --log-directory=/home/opengauss/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=full1.log \
    --note='full1'

[opengauss@d41ae4f5cf7b ~]$ gs_probackup show -B /home/opengauss/backup_1/

BACKUP INSTANCE 'gs_master_backup_1'
=====================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode    WAL Mode  TLI    Time   Data   WAL  Zratio  Start LSN  Stop LSN   Type  Status
=====================================================================================================================================================
 gs_master_backup_1  9.2      SRRSXN  2025-02-16 17:56:13+08  FULL    STREAM    1/0      6s  650MB  16MB    1.00  0/D000028  0/D0001E8  FILE  OK


gs_probackup backup -B /home/opengauss/backup_1 --instance gs_master_backup_1 -b full -D /var/lib/opengauss/data --progress \
    --log-directory=/home/opengauss/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=full2.log \
    --note='full2'

[opengauss@d41ae4f5cf7b ~]$ gs_probackup show -B /home/opengauss/backup_1/
BACKUP INSTANCE 'gs_master_backup_1'
=====================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode    WAL Mode  TLI    Time   Data   WAL  Zratio  Start LSN  Stop LSN   Type  Status
=====================================================================================================================================================
 gs_master_backup_1  9.2      SRRT7N  2025-02-16 18:02:12+08  FULL    STREAM    1/0      5s  667MB  16MB    0.98  0/F000028  0/F000448  FILE  OK
 gs_master_backup_1  9.2      SRRSXN  2025-02-16 17:56:13+08  FULL    STREAM    1/0      6s  650MB  16MB    1.00  0/D000028  0/D0001E8  FILE  OK


gdb --args gs_probackup backup -B /home/opengauss/backup_1 --instance gs_master_backup_1 -b PTRACK -D /var/lib/opengauss/data --progress \
    --log-directory=/home/opengauss/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=inc1.log \
    --note='inc1'
backup_files

TRUNCATE TABLE warehouse_t1;

BACKUP INSTANCE 'gs_master_backup_1'
=======================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode    WAL Mode  TLI    Time   Data   WAL  Zratio  Start LSN   Stop LSN    Type  Status
=======================================================================================================================================================
 gs_master_backup_1  9.2      SRRV6P  2025-02-16 18:48:47+08  PTRACK  STREAM    1/1   4m:3s  304MB  16MB    0.95  0/11000028  0/11298920  FILE  OK

gs_probackup restore -B /home/opengauss/backup_1/ --instance=gs_master_backup_1 -D /home/opengauss/restore_data_test/ -i SRRV6P --progress -j 4

gs_ctl start -D /home/opengauss/restore_data_test/ -w -t 3600


[opengauss@d41ae4f5cf7b ~]$ gs_probackup show -B /home/opengauss/backup_1/
BACKUP INSTANCE 'gs_master_backup_1'
======================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode    WAL Mode  TLI   Time   Data   WAL  Zratio  Start LSN   Stop LSN    Type  Status
======================================================================================================================================================
 gs_master_backup_1  9.2      SRRZJV  2025-02-16 20:25:12+08  PTRACK  STREAM    1/1  6m:9s  287MB  32MB    1.00  0/18000028  0/190C16A0  FILE  OK
 gs_master_backup_1  9.2      SRRZ2B  2025-02-16 20:08:37+08  FULL    STREAM    1/0     6s  689MB  16MB    0.98  0/15000028  0/15017348  FILE  OK


gs_probackup restore -B /home/opengauss/backup_1/ --instance=gs_master_backup_1 -D /home/opengauss/restore_data_test/ -i SRRZJV --progress -j 4

gs_ctl start -D /home/opengauss/restore_data_test/ -w -t 3600




CREATE TABLE warehouse_t1 (
    W_WAREHOUSE_SK            INTEGER               NOT NULL,
    W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
    W_WAREHOUSE_NAME          VARCHAR(20)                   ,
    W_WAREHOUSE_SQ_FT         INTEGER                       ,
    W_STREET_NUMBER           CHAR(10)                      ,
    W_STREET_NAME             VARCHAR(60)                   ,
    W_STREET_TYPE             CHAR(15)                      ,
    W_SUITE_NUMBER            CHAR(10)                      ,
    W_CITY                    VARCHAR(60)                   ,
    W_COUNTY                  VARCHAR(30)                   ,
    W_STATE                   CHAR(2)                       ,
    W_ZIP                     CHAR(10)                      ,
    W_COUNTRY                 VARCHAR(20)                   ,
    W_GMT_OFFSET              DECIMAL(5,2)
);
ALTER TABLE warehouse_t1 ADD COLUMN insert_time TIMESTAMP WITH TIME ZONE DEFAULT NOW();

tpcds=# select oid,relfilenode,relname from pg_class where relname='warehouse_t1';
  oid  | relfilenode |   relname
-------+-------------+--------------
 24590 |       24590 | warehouse_t1
(1 row)



CREATE TABLE warehouse_t2 (
    W_WAREHOUSE_SK            INTEGER               NOT NULL,
    W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
    W_WAREHOUSE_NAME          VARCHAR(20)                   ,
    W_WAREHOUSE_SQ_FT         INTEGER                       ,
    W_STREET_NUMBER           CHAR(10)                      ,
    W_STREET_NAME             VARCHAR(60)                   ,
    W_STREET_TYPE             CHAR(15)                      ,
    W_SUITE_NUMBER            CHAR(10)                      ,
    W_CITY                    VARCHAR(60)                   ,
    W_COUNTY                  VARCHAR(30)                   ,
    W_STATE                   CHAR(2)                       ,
    W_ZIP                     CHAR(10)                      ,
    W_COUNTRY                 VARCHAR(20)                   ,
    W_GMT_OFFSET              DECIMAL(5,2)
);
ALTER TABLE warehouse_t2 ADD COLUMN insert_time TIMESTAMP WITH TIME ZONE DEFAULT NOW();

tpcds=# select oid,relfilenode,relname from pg_class where relname='warehouse_t2';
  oid  | relfilenode |   relname
-------+-------------+--------------
 24594 |       24594 | warehouse_t2
(1 row)

./update2_data.sh
./update1_data.sh

[opengauss@d41ae4f5cf7b ~]$ ls -alh /var/lib/opengauss/data/base/16389/2459*
-rw------- 1 opengauss opengauss 72K Feb 17 05:33 /var/lib/opengauss/data/base/16389/24590
-rw------- 1 opengauss opengauss 24K Feb 17 05:33 /var/lib/opengauss/data/base/16389/24590_fsm
-rw------- 1 opengauss opengauss 80K Feb 17 05:33 /var/lib/opengauss/data/base/16389/24594
-rw------- 1 opengauss opengauss 24K Feb 17 05:33 /var/lib/opengauss/data/base/16389/24594_fsm


gs_probackup backup -B /home/opengauss/backup_1 --instance gs_master_backup_1 -b full -D /var/lib/opengauss/data --progress \
    --log-directory=/home/opengauss/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=full_backup_1.log \
    --note='full backup 1'

[opengauss@d41ae4f5cf7b ~]$ gs_probackup show -B /home/opengauss/backup_1/
BACKUP INSTANCE 'gs_master_backup_1'
======================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode    WAL Mode  TLI   Time   Data   WAL  Zratio  Start LSN   Stop LSN    Type  Status
======================================================================================================================================================
 gs_master_backup_1  9.2      SRSPYL  2025-02-17 05:49:35+08  FULL    STREAM    1/0     8s  716MB  16MB    0.98  0/1D000028  0/1D01B808  FILE  OK
 gs_master_backup_1  9.2      SRRZJV  2025-02-16 20:25:12+08  PTRACK  STREAM    1/1  6m:9s  287MB  32MB    1.00  0/18000028  0/190C16A0  FILE  OK
 gs_master_backup_1  9.2      SRRZ2B  2025-02-16 20:08:37+08  FULL    STREAM    1/0     6s  689MB  16MB    0.98  0/15000028  0/15017348  FILE  OK

kill update1_data.sh

gdb --args gs_probackup backup -B /home/opengauss/backup_1 --instance gs_master_backup_1 -b PTRACK -D /var/lib/opengauss/data --progress \
    --log-directory=/home/opengauss/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=inc_backup_1.log \
    --note='inc backup 1'
backup_files

./update1_data.sh
kill update1_data.sh

TRUNCATE TABLE warehouse_t1;

[opengauss@d41ae4f5cf7b ~]$ ls -alh /var/lib/opengauss/data/base/16389/2459*
-rw------- 1 opengauss opengauss    0 Feb 17 06:09 /var/lib/opengauss/data/base/16389/24590
-rw------- 1 opengauss opengauss 2.3M Feb 17 06:09 /var/lib/opengauss/data/base/16389/24594
-rw------- 1 opengauss opengauss  24K Feb 17 06:09 /var/lib/opengauss/data/base/16389/24594_fsm
-rw------- 1 opengauss opengauss 8.0K Feb 17 05:44 /var/lib/opengauss/data/base/16389/24594_vm
-rw------- 1 opengauss opengauss    0 Feb 17 06:09 /var/lib/opengauss/data/base/16389/24598
[opengauss@d41ae4f5cf7b ~]$ ls -alh /var/lib/opengauss/data/base/16389/2459*
-rw------- 1 opengauss opengauss 2.3M Feb 17 06:10 /var/lib/opengauss/data/base/16389/24594
-rw------- 1 opengauss opengauss  24K Feb 17 06:10 /var/lib/opengauss/data/base/16389/24594_fsm
-rw------- 1 opengauss opengauss 8.0K Feb 17 05:44 /var/lib/opengauss/data/base/16389/24594_vm
-rw------- 1 opengauss opengauss    0 Feb 17 06:09 /var/lib/opengauss/data/base/16389/24598

tpcds=# select oid,relfilenode,relname from pg_class where relname='warehouse_t1';
  oid  | relfilenode |   relname
-------+-------------+--------------
 24590 |       24598 | warehouse_t1
(1 row)


[opengauss@d41ae4f5cf7b ~]$ gs_probackup show -B /home/opengauss/backup_1/

BACKUP INSTANCE 'gs_master_backup_1'
=======================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode    WAL Mode  TLI    Time   Data   WAL  Zratio  Start LSN   Stop LSN    Type  Status
=======================================================================================================================================================
 gs_master_backup_1  9.2      SRSQO0  2025-02-17 06:09:56+08  PTRACK  STREAM    1/1  5m:13s  308MB  32MB    0.95  0/22000708  0/230F71B8  FILE  OK
 gs_master_backup_1  9.2      SRSPYL  2025-02-17 05:49:35+08  FULL    STREAM    1/0      8s  716MB  16MB    0.98  0/1D000028  0/1D01B808  FILE  OK
 gs_master_backup_1  9.2      SRRZJV  2025-02-16 20:25:12+08  PTRACK  STREAM    1/1   6m:9s  287MB  32MB    1.00  0/18000028  0/190C16A0  FILE  OK
 gs_master_backup_1  9.2      SRRZ2B  2025-02-16 20:08:37+08  FULL    STREAM    1/0      6s  689MB  16MB    0.98  0/15000028  0/15017348  FILE  OK

gs_probackup restore -B /home/opengauss/backup_1/ --instance=gs_master_backup_1 -D /home/opengauss/restore_data/ -i SRSQO0 --progress -j 4

cp backup_1/backups/gs_master_backup_1/SRSPYL/database/base/16389/24590* restore_data/base/16389/

vim /home/opengauss/restore_data/postgresql.conf
port = 15432

gs_ctl start -D /home/opengauss/restore_data/ -w -t 3600

select pg_enable_delay_ddl_recycle();
select pg_disable_delay_ddl_recycle('00000000/244ACD78', false);
```


- 
```shell
mkdir -p /home/omm/backup_1/backups
mkdir -p /home/omm/backup_1/wal
gs_probackup add-instance -B /home/omm/backup_1 -D /var/lib/opengauss/data --instance gs_master_backup_1

gs_probackup backup -B /home/omm/backup_1 --instance gs_master_backup_1 -b full -D /var/lib/opengauss/data --progress \
    --log-directory=/home/omm/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=full1.log \
    --note='full1'

gs_probackup show -B /home/omm/backup_1/
BACKUP INSTANCE 'gs_master_backup_1'
============================================================================================================================================================
 Instance            Version  ID      Recovery Time           Mode  WAL Mode  TLI  Time   Data   WAL  Zratio  Start LSN  Stop LSN   Type  S3 Status  Status
============================================================================================================================================================
 gs_master_backup_1  9.2      SRX0AA  2025-02-19 05:23:00+00  FULL  STREAM    1/0    5s  644MB  16MB    1.00  0/3000028  0/30001E8  FILE  UNKNOWN    OK

gs_probackup restore -B /home/omm/backup_1/ --instance=gs_master_backup_1 -D /home/omm/restore_data/ -i SRX0AA --progress -j 4

gs_ctl start -D /home/omm/restore_data/ -w -t 3600
gs_ctl stop -D /home/omm/restore_data/ -w -t 3600

/usr/local/opengauss/bin/gaussdb -D /home/omm/restore_data

LD_LIBRARY_PATH=/lib gdb --args /usr/local/opengauss/bin/gaussdb -D /home/omm/restore_data

set environment LD_LIBRARY_PATH=/usr/local/opengauss/lib
```
