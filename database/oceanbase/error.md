# error

- observer 崩溃了
```shell
su admin
cd /home/admin/oceanbase/
/home/admin/oceanbase/bin/observer
```

- ERROR 4656 (HY000): resource pool unit num is bigger than zone server count
```shell
或许是有节点挂了
```

- error while loading shared libraries: libidn.so.11: cannot open shared object file: No such file or directory
```shell
去找一个不就好
```


- 内容 
[2023-11-01 16:54:32.969724] INFO  [STORAGE] ob_pg_sstable_garbage_collector.cpp:195 [2275][0][Y0-0000000000000000-0-0] [lt=19] [dc=0] do one gc free sstable by queue(ret=0, free sstable cnt=0)
[2023-11-01 16:54:32.973866] WARN  do_write (ob_storage_oss_base.cpp:2274) [3257][0][Y0-0000000000000000-0-0] [lt=19] [dc=0] oss object not match(ret=-9003, object_type="Normal")
[2023-11-01 16:54:32.973887] WARN  do_write (ob_storage_oss_base.cpp:2283) [3257][0][Y0-0000000000000000-0-0] [lt=18] [dc=0] position and offset do not match(ret=-9003, position=0, offset=9885)
[2023-11-01 16:54:32.973895] WARN  pwrite (ob_storage_oss_base.cpp:2204) [3257][0][Y0-0000000000000000-0-0] [lt=5] [dc=0] failed to do write(ret=-9003, buf="AB", size=9913, offset=9885)
[2023-11-01 16:54:32.973901] WARN  [STORAGE] pwrite (ob_storage.cpp:1492) [3257][0][Y0-0000000000000000-0-0] [lt=5] [dc=0] failed to write(ret=-9003)
[2023-11-01 16:54:32.973910] WARN  [ARCHIVE] push_log (ob_archive_io.cpp:343) [3257][0][Y0-0000000000000000-0-0] [lt=4] [dc=0] storage appender write fail(ret=-9003, uri=oss://wdg1/wdg80140141142/1675402848/incarnation_1/1001/clog/3/data/1100611139404002/0/97, storage_info_=host=192.168.78.212&access_id=123&access_key=123, data="AB", data_len=9913)
```shell
oss 协议
返回偏移量有问题
```

- (4179, 'restore tenant when restore_concurrency is 0 not allowed')
```shell

```
