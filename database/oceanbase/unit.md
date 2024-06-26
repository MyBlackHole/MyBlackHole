# unit

资源单元

```shell
CREATE RESOURCE UNIT unitname 
MAX_CPU [=] cpunum, 
MAX_MEMORY [=] memsize, 
MAX_IOPS [=] iopsnum, 
MAX_DISK_SIZE [=] disksize, 
MAX_SESSION_NUM [=] sessionnum, 
[MIN_CPU [=] cpunum,]
[MIN_MEMORY [=] memsize,] 
[MIN_IOPS [=] iopsnum] ;
```

- 查询单元信息
```sql
select * from __all_unit_config;
SELECT * FROM oceanbase.gv$unit;
```

- 查看单元配置
```sql
MySQL [oceanbase]> SELECT * FROM oceanbase.__all_unit_config;
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| gmt_create                 | gmt_modified               | unit_config_id | name            | max_cpu | min_cpu | max_memory | min_memory | max_iops | min_iops | max_disk_size | max_session_num     |
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| 2023-06-29 10:03:47.971740 | 2023-06-29 10:03:47.971740 |              1 | sys_unit_config |       5 |     2.5 | 5798205849 | 4831838208 |    10000 |     5000 |  320902004736 | 9223372036854775807 |
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
1 row in set (0.01 sec)
```

- 创建资源，绑定数据池
```shell
<!-- 创建资源单元 -->
obclient> CREATE RESOURCE UNIT unit1 MAX_CPU 4, MAX_MEMORY '1G', MAX_IOPS 128,MAX_DISK_SIZE '1G', MAX_SESSION_NUM 64, MIN_CPU=4, MIN_MEMORY= '1G', MIN_IOPS=128;

MySQL [oceanbase]> SELECT * FROM oceanbase.__all_unit_config;
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| gmt_create                 | gmt_modified               | unit_config_id | name            | max_cpu | min_cpu | max_memory | min_memory | max_iops | min_iops | max_disk_size | max_session_num     |
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| 2023-06-29 10:03:47.971740 | 2023-06-29 10:03:47.971740 |              1 | sys_unit_config |       5 |     2.5 | 5798205849 | 4831838208 |    10000 |     5000 |  320902004736 | 9223372036854775807 |
| 2023-10-12 09:15:42.105924 | 2023-10-12 09:15:42.105924 |           1001 | unit1           |       4 |       4 | 5368709120 | 5368709120 |      128 |      128 |   10737418240 |                  64 |
+----------------------------+----------------------------+----------------+-----------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
2 rows in set (0.00 sec)

<!-- 设置数据池的资源单元 -->
obclient> ALTER RESOURCE POOL pool1 UNIT='unit1';

<!-- 删除资源单元 -->
obclient> DROP RESOURCE UNIT unit1;
DROP RESOURCE UNIT unit2;




CREATE RESOURCE UNIT unit1 MAX_CPU 1, MAX_MEMORY '5G', MAX_IOPS 128,MAX_DISK_SIZE '1G', MAX_SESSION_NUM 64, MIN_CPU=1, MIN_MEMORY= '5G', MIN_IOPS=128;
```


- 修改资源
```shell
alter resource unit sys_unit_config max_disk_size=220902004736;
```
