# tenant

- 租户创建
```shell
CREATE TENANT IF NOT EXISTS tenant1 CHARSET='utf8mb4', ZONE_LIST=('zone1','zone2','zone3'), PRIMARY_ZONE='zone1;zone2,zone3', RESOURCE_POOL_LIST=('pool1');

CREATE TENANT IF NOT EXISTS tenant1 CHARSET='utf8mb4',  RESOURCE_POOL_LIST=('pool_wdg_t_1_zone2_eex');

oceanbase> CREATE TENANT IF NOT EXISTS tenant2 CHARSET='utf8mb4',  RESOURCE_POOL_LIST=('pool1');
```

- 查看租户信息
```shell
SELECT * FROM oceanbase.gv$tenant;
```

- 删除租户
```shell
DROP TENANT tenant_name [FORCE | PURGE]

DROP TENANT t1 FORCE;
```

- 查看租户物理元数据信息
```shell
select * from __all_tenant_meta_table;
```

- 修改密码
```shell
<!-- 修改登陆用户 -->
SET PASSWORD = PASSWORD('password');

<!-- 修改指定用户(当前用户必须拥有全局的CREATE USER权限) -->
SET PASSWORD FOR sqluser01 = password('***1**'); 
```
