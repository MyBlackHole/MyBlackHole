# backup

- 查看备份配置
```shell
obclient> SHOW PARAMETERS LIKE 'backup_dest'\G
***************************[ 1. row ]***************************
zone       | zone1
svr_type   | observer
svr_ip     | 192.168.80.140
svr_port   | 2882
name       | backup_dest
data_type  | <null>
value      | file:///volmountpoint/aiopool/1d6ad9a83ab6_0_5_1688011161_oceanbase
info       | backup dest
section    | OBSERVER
scope      | CLUSTER
source     | DEFAULT
edit_level | DYNAMIC_EFFECTIVE
***************************[ 2. row ]***************************
zone       | zone3
svr_type   | observer
svr_ip     | 192.168.80.142
svr_port   | 2882
name       | backup_dest
data_type  | <null>
value      | file:///volmountpoint/aiopool/1d6ad9a83ab6_0_5_1688011161_oceanbase
info       | backup dest
section    | OBSERVER
scope      | CLUSTER
source     | DEFAULT
edit_level | DYNAMIC_EFFECTIVE
***************************[ 3. row ]***************************
zone       | zone2
svr_type   | observer
svr_ip     | 192.168.80.141
svr_port   | 2882
name       | backup_dest
data_type  | <null>
value      | file:///volmountpoint/aiopool/1d6ad9a83ab6_0_5_1688011161_oceanbase
info       | backup dest
section    | OBSERVER
scope      | CLUSTER
source     | DEFAULT
edit_level | DYNAMIC_EFFECTIVE
```

- 强制停止备份相关任务
```shell
ALTER SYSTEM CANCEL ALL BACKUP FORCE;
```

- 配置 s3
```shell
ALTER SYSTEM SET backup_dest='oss://mybucket?host=192.168.30.126&access_id=bNkZp8WKDMziff8x6hOz&access_key=OvygWaWLqxDBmCeMth3PovD12p4DZL1I5eSLNGD3';
ALTER SYSTEM SET backup_dest='oss://wdg1?host=192.168.30.126&access_id=1&access_key=1';
ALTER SYSTEM SET backup_dest='oss://oceanbase-shunde?host=zeyang@1510629197150037.onaliyun.com&access_id=LTAI5tMQYtZkbVMN2vZckpAs&access_key=zqCmshOoLs8BITRgMelbVOU73VM40i';
oceanbase> ALTER SYSTEM SET backup_dest='oss://backuptest/test1?host=192.168.30.126&access_id=root&access_key=123456';



ALTER SYSTEM SET backup_dest='file:///obtest/archive';
```

- 发起日志归档
```shell
alter system archivelog;
```

- 查看日志归档进度
```shell
SELECT * FROM CDB_OB_BACKUP_ARCHIVELOG;
```

- 关闭日志归档
```shell
alter system noarchivelog;
```

- 发起数据备份
```shell
alter system backup database;
```

- 查看备份进度
```shell
USE oceanbase;
SELECT * FROM CDB_OB_BACKUP_PROGRESS;
```

- 备份进度
```shell
SELECT * FROM CDB_OB_BACKUP_BACKUPSET_JOB;
```

- 查看备份历史
```shell
USE oceanbase;
SELECT * FROM CDB_OB_BACKUP_SET_DETAILS;
```

