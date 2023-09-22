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

- 
```shell
SELECT * FROM oceanbase.gv$unit;
```


```sql
SELECT * FROM oceanbase.__all_unit_config;
```

```shell
obclient> CREATE RESOURCE UNIT unit2 MAX_CPU 4, MAX_MEMORY '5G', MAX_IOPS 128,MAX_DISK_SIZE '10G', MAX_SESSION_NUM 64, MIN_CPU=4, MIN_MEMORY= '5G', MIN_IOPS=128;

obclient> ALTER RESOURCE POOL pool1 UNIT='unit2';

obclient> DROP RESOURCE UNIT unit1;
```
