# pool

资源池

```shell
CREATE RESOURCE POOL poolname 
UNIT [=] unitname,
UNIT_NUM [=] unitnum, 
ZONE_LIST [=] ('zone' [, 'zone'...]);
```

```sql
obclient> CREATE RESOURCE POOL pool1 UNIT='unit1',UNIT_NUM=1,ZONE_LIST=('zone1','zone2','zone3');

oceanbase> CREATE RESOURCE POOL pool1 UNIT='config_wdg_t_1_zone3_aio_pcr',UNIT_NUM=1,ZONE_LIST=('zone1','zone3');
```

- 删除
```shell
obclient> DROP RESOURCE POOL pool_name;
```
