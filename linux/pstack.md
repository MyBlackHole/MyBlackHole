# pstack

查看程序进程栈

## 例子

- 程序进程栈

```shell
[root@dengxizz-worker2 fsclient]# pstack 78238
#0  0x00007fa472257740 in __read_nocancel () from /lib64/libpthread.so.0
#1  0x000000000043e276 in UpdateFileBlock (backupPath="/app/pgBa/192.168.50.89/hA1-I1/_data_pgdata", verStorePath="/app/pgBa/192.168.50.89/hA1_full/_data_pgdata/data", opFile="/data/pgdata/global/pg_control") at restore.cpp:203
#2  0x000000000043d799 in RestoreTargetPath (backupPath="/app/pgBa/192.168.50.89/hA1-I1/_data_pgdata", verStorePath="/app/pgBa/192.168.50.89/hA1_full/_data_pgdata/data", bFullBak=false) at restore.cpp:73
#3  0x000000000043f92a in RestoreBackup (backName="hA1-I1", backupPathKey="_data_pgdata", savePath="/app/pgBa/192.168.50.89", targetPath="/app/pgBa/192.168.50.89/hA1_full/_data_pgdata/data", restoreType=0, shortPath=0 '\000') at restore.cpp:441
#4  0x000000000040df65 in restore_backup (pConfig=0x67bb00 <g_Config>, buff=0x7fa471275010 "", buflen=10485760) at cli.cpp:736
#5  0x0000000000407944 in main (argc=9, argv=0x7ffd037027b8) at main.cpp:379
```
