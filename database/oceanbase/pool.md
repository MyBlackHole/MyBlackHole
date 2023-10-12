# pool

资源池

## 创建

- 语法
```shell
CREATE RESOURCE POOL poolname 
UNIT [=] unitname,
UNIT_NUM [=] unitnum, 
ZONE_LIST [=] ('zone' [, 'zone'...]);
```

## 修改

- 语法
```shell
ALTER RESOURCE UNIT unitname 
MAX_CPU [=] cpunum, 
MAX_MEMORY [=] memsize, 
MAX_IOPS [=] iopsnum, 
MAX_DISK_SIZE [=] disksize, 
MAX_SESSION_NUM [=] sessionnum, 
[MIN_CPU [=] cpunum,]
[MIN_MEMORY [=] memsize,] 
[MIN_IOPS [=] iopsnum] ;
```

- 查询资源池
```shell
SELECT * FROM oceanbase.__all_resource_pool;
```

- 创建数据池
```sql
obclient> CREATE RESOURCE POOL pool1 UNIT='unit1',UNIT_NUM=1,ZONE_LIST=('zone1','zone2','zone3');

oceanbase> CREATE RESOURCE POOL pool1 UNIT='config_wdg_t_1_zone3_aio_pcr',UNIT_NUM=1,ZONE_LIST=('zone1','zone3');
```

- 删除
```shell
obclient> DROP RESOURCE POOL pool_name;
```
