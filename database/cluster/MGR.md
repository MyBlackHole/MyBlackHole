# MGR （MySQL Group Replication）

使用一致性算法

![[imgs/MGR.png]]

## 支持多主模式，但官方推荐单主模式

多主模式下，客户端可以随机向MySQL节点写入数据

单主模式下，MGR集群会选出primary节点负责写请求，primary节点与其它节点都可以进行读请求处理.

```
// 查看MGR的组员
select * from performance_schema.replication_group_members;
// 查看MGR的状态
select * from performance_schema.replication_group_member_stats;
// 查看MGR的一些变量
show variables like 'group%';
// 查看服务器是否只读
show variables like 'read_only%';
```

优点:
- 基本无延迟，延迟比异步的小很多
- 支持多写模式，但是目前还不是很成熟
- 数据的强一致性，可以保证数据事务不丢失

缺点:
- 仅支持innodb
- 只能用在GTID模式下，且日志格式为row格式

适用的业务场景：
- 对主从延迟比较敏感
- 希望对对写服务提供高可用，又不想安装第三方软件
- 数据强一致的场景
