# MHA （MySQL Master High Availability）
Mysql Master 高可用

![[imgs/Pasted image 20230402113645.png]]

| 资源     | 数量  | 说明                                       |
|-------------- | -------------- | -------------- |
| 主DB     | 2     | 用于主备模式的主主复制                     |
| 从DB     | 0-N台 | 可以根据需要配置N台从服务器                |
| ip地址   | n+2   | N为Myql服务器的数量                        |
| 监控用户 | 1     | 用户监控数据库状态的MySQL用户(replication) |
| 复制用户 | 1     | 用于配置MySQL复制的用户                    |


## MHA采用的是从slave中选出Master，故障转移

- 从服务器
    - 选举具有最新更新的slave
    - 尝试从宕机的master中保存二进制日志
    - 应用差异的中继日志到其它的slave
    - 应用从master保存的二进制日志
    - 提升选举的slave为master
    - 配置其它的slave向新的master同步

- 优点
    - MHA除了支持日志点的复制还支持GTID的方式
    - 同MMM相比，MHA会尝试从旧的Master中恢复旧的二进制日志，只是未必每次都能成功。如果希望更少的数据丢失场景，建议使用MHA架构。

- 缺点
    - MHA需要自行开发VIP转移脚本。
    - MHA只监控Master的状态，未监控Slave的状态
