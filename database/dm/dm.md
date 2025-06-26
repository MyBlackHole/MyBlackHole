# dm

```shell
podman run -d -p 5236:5236 --name dm8 \
--restart=always \
-e PAGE_SIZE=16 \
-e UNICODE_FLAG=1 \
-e LENGTH_IN_CHAR=1 \
-e CASE_SENSITIVE=0 \
-e SYSDBA_PWD='123abc!@#' \
-e LD_LIBRARY_PATH=/opt/dmdbms/bin \
-e INSTANCE_NAME=dm8_instance \
-v /opt/aio/dm8:/opt/dmdbms/data \
sizx/dm8:1-2-128-22.08.04-166351-20005-CTM

❯ podman logs -f dm8
file dm.key not found, use default license!
License will expire in 14 day(s) on 2025-06-25
Normal of FAST
Normal of DEFAULT
Normal of RECYCLE
Normal of KEEP
Normal of ROLL

 log file path: /opt/dmdbms/data/DAMENG/DAMENG01.log


 log file path: /opt/dmdbms/data/DAMENG/DAMENG02.log

write to dir [/opt/dmdbms/data/DAMENG].
create dm database success. 2025-06-11 09:56:07
initdb V8
db version: 0x7000c
Init DM success!
Start DmAPService...
Starting DmAPService:                                      [ OK ]
/opt/dmdbms/conf/dm.ini does not exist, use default dm.ini
Start DMSERVER success!
Dmserver is running.
DM Database is not OK, please wait...
DM Database is OK
Finished soft link DM current dm_DMSERVER_202506.log to dm_DMSERVER.log
 * Starting periodic command scheduler cron
   ...done.


cd /opt/dmdbms/bin
./disql sysdba
password:123abc!@#

SQL> select ID_CODE();

LINEID     ID_CODE()
---------- -----------------------------------------
1          --08134283904-20220804-166351-20005 Pack4

used time: 3.590(ms). Execute id is 56300.

```


- 归档
```shell
alter system switch logfile;
BACKUP ARCHIVE LOG FROM LSN 1968716 BACKUPSET '/volmountpoint/arch/log_1' device type tape BACKUPINFO 'test archive log backup';
```

- 备份
```shell
<!-- 全量备份 -->
SQL> backup database full to full1 backupset '/volmountpoint/full/full1';
<!-- 增量备份 -->
SQL> backup database increment to inc1 backupset '/volmountpoint/inc/inc1';
<!-- 基于全量备份做增量备份 -->
SQL> backup database increment BASE ON BACKUPSET '/volmountpoint/full/full1' to incrbk1 backupset '/volmountpoint/inc/inc1';

```

- test
```shell
SQL> backup database full to full2 backupset '/volmountpoint/full/full2';

export LD_LIBRARY_PATH=/opt/dmdbms/bin:$LD_LIBRARY_PATH
SQL> backup database to full3 backupset '/volmountpoint/full/full3';

SQL> backup database full to full4 backupset '/volmountpoint/full/full4';

backup database full to full2 backupset '/full/full2';

<!-- 使用 tape 设备备份(libdmsbtex.so) -->
backup database to full_3 backupset '/full/full3' device type tape BACKUPINFO 'test base backup';

backup database to full_3 backupset '/opt/aio/dm8/bak/full/full3' device type tape BACKUPINFO 'test base backup';
```
