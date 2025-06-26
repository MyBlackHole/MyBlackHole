# error

- No local or remote archive log.
```shell

SQL> select arch_mode from v$database;

LINEID     arch_mode
---------- ---------
1          N


ALTER DATABASE MOUNT;
ALTER DATABASE ADD ARCHIVELOG 'DEST = /home/dm8/arch, TYPE = local,FILE_SIZE = 1024, SPACE_LIMIT = 2048';
ALTER DATABASE ARCHIVELOG;
ALTER DATABASE OPEN;

SQL> select arch_mode from v$database;

LINEID     arch_mode
---------- ---------
1          Y

<!-- shutdown immediate; -->
```

- [-718]:Archive log collected not consecutive.
```shell
./disql sysdba
shutdown immediate;

/opt/dmdbms/data/DAMENG/dm.ini

root@03504eb277fb:/opt/dmdbms/bin# ./dmrman
dmrman V8
RMAN> repair archivelog database '/opt/dmdbms/data/DAMENG/dm.ini';
repair archivelog database '/opt/dmdbms/data/DAMENG/dm.ini';
file dm.key not found, use default license!
Database mode = 0, oguid = 0
Normal of FAST
Normal of DEFAULT
Normal of RECYCLE
Normal of KEEP
Normal of ROLL
EP[0]'s cur_lsn[41906], file_lsn[41906]
repair archive log successfully.
repair time used: 200.344(ms)
time used: 201.764(ms)
RMAN> exit
time used: 0.525(ms)

./dmserver /opt/dmdbms/data/DAMENG/dm.ini -noconsole
```
