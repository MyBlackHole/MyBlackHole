# tenant

- 租户创建
```sql
CREATE TENANT IF NOT EXISTS tenant1 CHARSET='utf8mb4', ZONE_LIST=('zone1','zone2','zone3'), PRIMARY_ZONE='zone1;zone2,zone3', RESOURCE_POOL_LIST=('pool1');

CREATE TENANT IF NOT EXISTS tenant1 CHARSET='utf8mb4',  RESOURCE_POOL_LIST=('pool_wdg_t_1_zone2_eex');

oceanbase> CREATE TENANT IF NOT EXISTS tenant2 CHARSET='utf8mb4',  RESOURCE_POOL_LIST=('pool1');
```

- 查看租户信息
```sql
SELECT * FROM oceanbase.gv$tenant;
```

- 删除租户
```sql
DROP TENANT tenant_name [FORCE | PURGE]

DROP TENANT t1 FORCE;
```
