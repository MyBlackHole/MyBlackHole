# backup
[细节](https://www.modb.pro/db/43807)

## 备份流程

1. 打开参数enable_cbm_tracking,跟踪数据页的变化
```shell
docker exec -it opengauss_master /bin/bash
su - omm
gsql

omm=# show enable_cbm_tracking;
 enable_cbm_tracking
---------------------
 off
(1 row)

omm=# alter system set enable_cbm_tracking=on;
ALTER SYSTEM SET
omm=# show enable_cbm_tracking;
 enable_cbm_tracking
---------------------
 on
(1 row)
```

2. 初始化备份目录(/home/omm/gs_bak2021)
```shell
omm@opengauss_master:~$ gs_probackup --version
gs_probackup (openGauss 5.0.0 build a07d57c3) compiled at 2023-03-29 03:09:38 commit 0 last mr

omm@opengauss_master:~$ gs_probackup init -B /home/omm/backup_1
INFO: Backup catalog '/home/omm/backup_1' successfully inited
omm@opengauss_master:~$ tree -L 3 /home/omm/backup_1/
/home/omm/backup_1/
├── backups
└── wal

2 directories, 0 files

## 对目录的初始化操作实际是在备份目录下创建backups/和wal/子目录，分别用于存放备份文件和WAL文件。
```

3. 添加备份实例
```shell
omm@opengauss_master:~$ gs_probackup add-instance -B /home/omm/backup_1 -D /var/lib/opengauss/data --instance gs_master_backup_1
INFO: Instance 'gs_master_backup_1' successfully inited


omm@opengauss_master:~$ gs_probackup show -B /home/omm/backup_1/

BACKUP INSTANCE 'gs_master_backup_1'
=======================================================================================================================
 Instance  Version  ID  Recovery Time  Mode  WAL Mode  TLI  Time  Data  WAL  Zratio  Start LSN  Stop LSN  Type  Status
=======================================================================================================================
```

4. 执行全量备份
```shell
gs_probackup backup -B /home/omm/backup_1 --instance gs_master_backup_1 -b full -D /var/lib/opengauss/data --progress \
    --log-directory=/home/omm/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=full_20230516.log \
    --retention-redundancy=2 \
    --compress  \
    --note='full backup test 1'

INFO: Backup start, gs_probackup version: 2.4.2, instance: gs_master_backup_1, backup ID: RUQME4, backup mode: FULL, wal mode: STREAM, remote: false, compress-algorithm: zlib, compress-level: 1
LOG: Backup destination is initialized
LOG: This openGauss instance was initialized with data block checksums. Data block corruption will be detected
INFO: Adding note to backup RUQME4: 'full backup test 1'
LOG: Database backup start
LOG: started streaming WAL at 0/5000000 (timeline 1)
[2023-05-16 06:08:29]: check identify system success
[2023-05-16 06:08:29]: send START_REPLICATION 0/5000000 success
[2023-05-16 06:08:29]: keepalive message is received
INFO: PGDATA size: 631MB
....
....
....
INFO: Backup RUQME4 data files are valid
INFO: Backup RUQME4 resident size: 612MB
INFO: Backup RUQME4 completed
```

5. 执行增量备份
- 测试数据 1
```shell
omm=# create table incr_bak1(name varchar(50));
CREATE TABLE
omm=# insert into incr_bak1 values('This is the first change.');
INSERT 0 1
omm=# select * from incr_bak1;
           name
---------------------------
 This is the first change.
(1 row)
```

- incremental 1
```shell
gs_probackup backup -B /home/omm/backup_1 --instance gs_master_backup_1 -b PTRACK -D /var/lib/opengauss/data --progress \
    --log-directory=/home/omm/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=incr1_20230516.log \
    --delete-expired --delete-wal \
    --retention-redundancy=2 \
    --compress  \
    --note='incremental backup test 1'

INFO: Backup start, gs_probackup version: 2.4.2, instance: gs_master_backup_1, backup ID: RUQNFT, backup mode: PTRACK, wal mode: STREAM, remote: false, compress-algorithm: zlib, compress-level: 1
LOG: Backup destination is initialized
LOG: This openGauss instance was initialized with data block checksums. Data block corruption will be detected
INFO: Adding note to backup RUQNFT: 'incremental backup test 1'
LOG: Database backup start
LOG: Latest valid FULL backup: RUQME4
INFO: Parent backup: RUQME4
LOG: started streaming WAL at 0/7000000 (timeline 1)
[2023-05-16 06:31:05]: check identify system success
[2023-05-16 06:31:05]: send START_REPLICATION 0/7000000 success
[2023-05-16 06:31:05]: keepalive message is received
[2023-05-16 06:31:05]: keepalive message is received
INFO: PGDATA size: 631MB
LOG: Current tli: 1
LOG: Parent start_lsn: 0/5000028
LOG: start_lsn: 0/7000028
INFO: Extracting pagemap of changed blocks
....
....
....

INFO: Backup RUQNFT data files are valid
INFO: Backup RUQNFT resident size: 273MB
INFO: Backup RUQNFT completed
LOG: REDUNDANCY=2
INFO: Evaluate backups by retention
INFO: Backup RUQNFT, mode: PTRACK, status: OK. Redundancy: 1/2, Time Window: 0d/0d. Active
INFO: Backup RUQME4, mode: FULL, status: OK. Redundancy: 1/2, Time Window: 0d/0d. Active
INFO: There are no backups to merge by retention policy
INFO: There are no backups to delete by retention policy
INFO: There is no WAL to purge by retention policy
```

- 测试数据 2
```shell
omm=# create table incr_bak2(name varchar(50));
CREATE TABLE
omm=# insert into incr_bak2 values('This is the second change.');
INSERT 0 1
omm=# select * from incr_bak2;
            name
----------------------------
 This is the second change.
(1 row)
```

- incremental 2
```shell
gs_probackup backup -B /home/omm/backup_1 --instance gs_master_backup_1 -b PTRACK -D /var/lib/opengauss/data --progress \
    --log-directory=/home/omm/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=incr1_20230516.log \
    --delete-expired --delete-wal \
    --retention-redundancy=2 \
    --compress  \
    --note='incremental backup test 2'


INFO: Backup start, gs_probackup version: 2.4.2, instance: gs_master_backup_1, backup ID: RUQNR3, backup mode: PTRACK, wal mode: STREAM, remote: false, compress-algorithm: zlib, compress-level: 1
LOG: Backup destination is initialized
LOG: This openGauss instance was initialized with data block checksums. Data block corruption will be detected
INFO: Adding note to backup RUQNR3: 'incremental backup test 2'
LOG: Database backup start
LOG: Latest valid FULL backup: RUQME4
INFO: Parent backup: RUQNFT
LOG: started streaming WAL at 0/9000000 (timeline 1)
[2023-05-16 06:37:51]: check identify system success
[2023-05-16 06:37:51]: send START_REPLICATION 0/9000000 success
[2023-05-16 06:37:51]: keepalive message is received
INFO: PGDATA size: 631MB
LOG: Current tli: 1
LOG: Parent start_lsn: 0/7000028
LOG: start_lsn: 0/9000028
INFO: Extracting pagemap of changed blocks
INFO: change bitmap start lsn location is 0/7000028
INFO: change bitmap end lsn location is 00000000/09000028
INFO: Pagemap successfully extracted, time elapsed: 0 sec
INFO: Start transferring data files
[2023-05-16 06:37:51]: keepalive message is received
....
....
....
INFO: Backup RUQNR3 data files are valid
INFO: Backup RUQNR3 resident size: 273MB
INFO: Backup RUQNR3 completed
LOG: REDUNDANCY=2
INFO: Evaluate backups by retention
INFO: Backup RUQNR3, mode: PTRACK, status: OK. Redundancy: 1/2, Time Window: 0d/0d. Active
INFO: Backup RUQNFT, mode: PTRACK, status: OK. Redundancy: 1/2, Time Window: 0d/0d. Active
INFO: Backup RUQME4, mode: FULL, status: OK. Redundancy: 1/2, Time Window: 0d/0d. Active
INFO: There are no backups to merge by retention policy
INFO: There are no backups to delete by retention policy
INFO: There is no WAL to purge by retention policy
```

6. 模拟删除整个数据库目录
```shell
# 停止数据库
omm@opengauss_master:~$ gs_ctl stop -D /var/lib/opengauss/data/
rm -rf /var/lib/opengauss/data/
```

- 全量恢复 (数据目录必须为空)
```shell
gs_probackup restore -B /home/omm/backup_1/ --instance=gs_master_backup_1 -D /var/lib/opengauss/data -i RUQME4 --progress -j 4    ##  -i 指定备份文件 ID,RUQME4 即为全备的ID
```

- 全量加一次增量 (数据目录必须为空)
```shell
gs_probackup restore -B /home/omm/backup_1/ --instance=gs_master_backup_1 -D /var/lib/opengauss/data -i RUQNFT --progress  ##  -i 指定备份文件 ID,RUQNFT 即第一次增备的ID
```

## 补充
1. 远程备份
```shell
# server 端 用户与权限要求
omm=# create user rep1 with sysadmin replication identified by 'gauss@123';  --rep1权限：sysadmin+replication
NOTICE:  The encrypted password contains MD5 ciphertext, which is not secure.
CREATE ROLE


omm=# \du rep1
                 List of roles
 Role name |      Attributes       | Member of
-----------+-----------------------+-----------
 rep1      | Replication, Sysadmin | {}

# 放开客户端对server端的连接
gs_guc reload -N all -I all -h "host all all 192.168.0.12/32 sha256"

# 放开rep1用户对server端的replication权限
gs_guc reload -N all -I all -h "host replication rep1 192.168.0.12/32 sha256"    

# client 配置
# 配置SSH互信
ssh-copy-id 192.168.0.12

# 配置SSH互信
ssh-copy-id stb1.opengauss.com

mkdir -p /home/omm/backup_2
gs_probackup init -B /home/omm/backup_2

gs_probackup add-instance -B /home/omm/backup_2 -D /var/lib/opengauss/data --instance='remote_backup_master_1' \
    --remote-host=192.168.0.11  \                  ## 远程Server主机
    --remote-port=22 \                             ## 远程连接端口(默认22端口)
    --remote-proto=ssh \                           ## 远程连接协议(默认ssh)
    --remote-path=/usr/local/opengauss/bin/ \      ## 远程Server主机的gs_probackup程序所在目录
    --remote-user=omm


gs_probackup backup -B /home/omm/backup_2 --instance=remote_backup_master_1 -b full -D /var/lib/opengauss/data \
    -h 192.168.0.11 -p 26000 -d postgres -U rep1 -W gauss@123 \
    --remote-host=192.168.0.11 \
    --remote-proto=ssh \
    --remote-port=22 \
    --remote-user=omm \
    --remote-path=/usr/local/opengauss/bin/
```
