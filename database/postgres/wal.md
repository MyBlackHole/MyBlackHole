# WAL

[推荐连接](https://www.modb.pro/db/114824)

## WAL 文件名解析

```shell
WAL segment file
WAL segment file文件长度为24,由3部分组成,每个部分是8个16进制数字:
1.第1部分是TimeLineID,0x00000000 -> 0xFFFFFFFF
2.第2部分是逻辑文件ID,0x00000000 -> 0xFFFFFFFF
3.第3部分是物理文件ID,0x00000000 -> 0x000000FF

逻辑文件ID占32bit,物理文件ID占8bit,16M的文件占24bit,合计64bit.PG通过这三部分的组合,达到最大64bit的文件寻址空间.


postgres@[local]:5432=#select pg_current_wal_lsn(),
postgres-#                   pg_walfile_name(pg_current_wal_lsn()),
postgres-#                   pg_walfile_name_offset(pg_current_wal_lsn());
 pg_current_wal_lsn |     pg_walfile_name      |       pg_walfile_name_offset
--------------------+--------------------------+------------------------------------
 0/19291E8          | 000000010000000000000001 | (000000010000000000000001,9605608)
(1 row)

 
LSN为0/19291E8
logId就是“0/19291E8”中的第一个数字，即“0”
logSeg就是“0/1B000108”中的第二个数字除以16M的大小，即19291E8除以16M，而16M相当于2的24次方，相当于十六进制数“19291E8”右移6位，即“19291E8”中的最高两位“01”
那么根据WAL文件的格式timelineID+logId+logSeg，则相当于:“00000001”+“00000000”+“00000001”，即为：“000000010000000000000001”
写的位置是在文件“000000010000000000000001”中的偏移量是多少呢？实际上是在“0/1B000108”中第二个数字“1B000108”后六位“9291E8”，换算成十进制为“9605608”。

postgres@[local]:5432=#select cast(cast(('x'||'009291E8') AS bit(32)) as int);
  int4
---------
 9605608
(1 row)


postgres@[local]:5432=#SELECT file_name, upper(to_hex(file_offset)) file_offset
postgres-# FROM pg_walfile_name_offset('0/19291E8');
        file_name         | file_offset
--------------------------+-------------
 000000010000000000000001 | 9291E8
(1 row)




timeline = xx
logno = (lsn-1)/(16M*256)
segno = (lsn-1)/16M%256

例如，LSN=0/1922E50, timeline=1
则，timeline = 1, logno=0, segno=1,其文件名即为
00000001 00000000 00000001
```

## sql 命令
```sql
SELECT pg_current_wal_lsn(); -- 查看当前 WAL 位置
SELECT pg_last_wal_receive_lsn(); -- 查看上次接收到的 WAL 位置
SELECT pg_last_wal_replay_lsn(); -- 查看上次重放的 WAL 位置

SELECT pg_switch_xlog(); -- 切换到下一个 WAL 文件
```

### gaussdb
```shell
postgres=> SELECT pg_current_xlog_location(); -- 查看当前事务日志位置 WAL 位置
 pg_current_xlog_location
--------------------------
 0/C3D77AE8
(1 row)

postgres=> SELECT pg_current_xlog_insert_location(); -- 查看当前事务日志插入位置
 pg_current_xlog_insert_location
---------------------------------
 0/C3DAB740
(1 row)

postgres=> SELECT gs_current_xlog_insert_end_location(); -- 查看当前事务日志插入结束位置
 gs_current_xlog_insert_end_location
-------------------------------------
 0/C3DD1398
(1 row)


postgres=> SELECT pg_switch_xlog(); -- 切换到下一个 WAL 文件
 pg_switch_xlog
-------------------------------------
 0/C3DF1398
(1 row)

postgres=> SELECT pg_cbm_tracked_location(); -- 查看当前 CBM 跟踪位置
 pg_cbm_tracked_location
-------------------------
 00000000/C3D89060
(1 row)
```
