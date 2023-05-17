# cbm
changed block map
对外提供数据页面的修改情况，并向提供外部接口，根据cbm信息可直接获取两次备份之间发生对于数据文件（行存、列存）的增量修改信息，并备份

当使用 roach 执行数据库实例的全量和增量备份时需要开启此参数，如果关闭会导致备份失败。
```shell
omm=# alter system set enable_cbm_tracking=on;
ALTER SYSTEM SET

omm=# show enable_cbm_tracking;
 enable_cbm_tracking
---------------------
 on
(1 row)
```

## 原理
[细节](https://www.jianshu.com/p/a126de7c085b)
