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


- 查询当前跟踪最新 lsn
```shell
gaussdb=# SELECT pg_cbm_tracked_location();
 pg_cbm_tracked_location
-------------------------
 00000000/1834A1D0
(1 row)
```

- 查询指定 lsn 范围修改块 map
```shell
gaussdb=# SELECT path,changed_block_number,changed_block_list FROM pg_cbm_get_changed_block('00000000/1833A1D0', '00000000/1834A1D0');
        path         | changed_block_number |                                                                        changed_block_list
---------------------+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------
 global/12636        |                    1 | 0
 base/12737/12131    |                   42 | 0, 1, 2, 6, 7, 18, 25, 27, 30, 32, 33, 34, 35, 36, 37, 40, 41, 43, 44, 45, 47, 48, 51, 52, 53, 54, 55, 56, 57, 58, 61, 62, 63, 65, 70, 80, 81, 85, 86, 87, 89, 90
 base/12737/12131_vm |                    1 | 0
 base/12737/12134    |                    3 | 5, 7, 10
 base/12737/12139    |                    1 | 974
 base/12737/12143    |                    1 | 475
 base/12737/12212    |                    1 | 48
 base/12737/12290    |                    1 | 1
(8 rows)
```
