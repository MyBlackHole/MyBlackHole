# GoldenDB

## GoldenDB整体架构

GoldenDB整体架构如下图所示包括：
    计算节点CN、数据节点DN、全局事务节点GTM、管理节点，其中管理节点分为集群管理和Insight运维管理平台
    集群管理部分包括元数据管理Metadataserver、ClusterManager和ProxyManager，这一部分实现分布式数据库架下的整个集群元数据、数据节点和计算节点的管理和维护
![[imgs/Pasted image 20230402183813.png]]


### OMM (Operations, Maintenance & Monitoring Manager）功能
是整个分布式数据库系统中用于进行维护工作的管理平台
负责所有组件的管理
主要功能包括用户和权限管理、统计监控、元数据管理、DBProxy管理、Cluster管理、操作日志查询、资源管理、FAQ管理、OMM系统配置、数据库备份管理、数据重分布等功能

### 管理节点
管理节点包含四个主要的功能模块：
1. MetaDataServer主要功能是管理分布式数据库的元数据信息，对外提供操作接口；持久化数据以及进行相应的任务管理工作。
2. ProxyManager（PM）主要功能包括：管理计算节点，管理连接实例，收集计算节点状态、统计告警信息和对计算节点的异常进行处理。
3. ClusterManager(CM) 在分布式数据库系统中主要用于存储节点安全组的管理，协同计算节点控制对数据库的访问。
4. LoadServer 主要功能是在存储节点间批量导入导出数据。

### 全局事务节点
全局事务协调中心，用于协助计算节点进行分布式事务管理，主要包括生成、释放全局事务ID（GTID）、维护活跃事务以及当前活跃GTIDs的快照
在GoldenDB中，只有跨分片的写操作才会申请GTID，其它读查询操作和单分片的写操作都不会申请GTID

### 计算节点
计算节点主要负责分布式优化、执行具体的分布式计划、分布式事务控制、存储节点负载均衡、用户认证与鉴权等任务

### 连接方式
应用客户端可以通过JDBC或者ODBC直接连接到计算节点，也可以经过负载均衡F5或loadbalance或LVS的方式连接到计算节点，达到流量均衡的目的

## GoldenDB组件和进程关系
- GoldenDB中各组件模块和进程列表

![[imgs/Pasted image 20230402184438.png]]

- 各个功能组件之间的访问关系如下图所示

![[imgs/Pasted image 20230402184501.png]]

1. 计算节点和数据节点之间通过dbagent建立长连接，所有的副本都需要建立
2. 数据节点主节点和从节点之间通过mysql的binlog同步复制原理，实现数据的同步
3. 事务访问计算节点，如果需要申请全局事务ID，会通过GTM管理节点申请GTID
4. 管理节点中ClusterManager会统一管理数据节点，比如数据节点的状态、扩容缩容，并协同计算节点控制数据的访问
5. 管理节点中的ProxyManager实现对计算节点的统一管理，会和每个计算节点进行连接
6. 管理节点中的Metadataserver管理元数据信息，有DDL相关的变更会在这里同步更新，元数据会保存在RDB中。同时为了优化执行效率，元数据信息也会同时同步到每个计算节点和数据节点的内存中，业务访问的时候优先从本地读取元数据信息。
7. 每台服务器会部署ommagent，用于执行OMM管理节点下发的命令，并将告警信息同步到OMM管理节点

- Insight运维管理平台访问关系

![[imgs/Pasted image 20230402184606.png]]

1. InsightAgent是主机代理，每台主机上部署，执行insightserver下发的命令，并将数据收集推送到kafka
2. Filebeat是日志采集代理，用于收集每台服务器的日志数据
3. 运维性能数据经过kafka消息队列后通过logstash采集到elasticsearch中存储
4. Insightserver会查询ES中的性能数据、RDB中的集群信息以及Redis中的缓存信息进行展示和汇总分析


- 启动
```shell
su - omm -c "ommtool -status" 
su - omm -c "ommtool -moni -start"

su - manager -c "dbstate"
su - manager -c "dbmoni -start"
```
