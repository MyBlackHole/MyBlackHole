# teledb
![[imgs/Pasted image 20230415190437.png]]
## 表类型
- 分片表
- 全局表
每个实例都有相同表结构、内容


## 配置

表配置:
/udal_cluster/tenant_1/dbproxy_cluster/dbproxy_cluster_0000000014/schemas/schema_0000000104/table/table_0000000142

事务配置：
/udal_cluster/tenant_1/dbproxy_cluster/dbproxy_cluster_0000000014/groups/group_0000000069/properties/transaction ^transaction

mysql 账户信息:
关系数据库 -- 安装部署 -- Set 部署 -- 密码更新

## 命令

- 查询全局序列
```shell
udal show sequence
```

- 查询命令列表
```shell
udal show help
```

- 查看服务器运行状态
```shell
udal show server
```

- 单点 mysql 连接
```shell
mysql --socket=/tmp/mysql_5555.sock -u RDS_agent -pReport25Gky8@#*
```
