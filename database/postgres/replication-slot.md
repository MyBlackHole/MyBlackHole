# Replication slot

复制槽

PG 复制槽用于记录主备流复制的状态，主要目的是防止 wal 日志被过早的删除，导致备库流复制中断。
复制槽是有状态的，能够持久化到磁盘上，允许宕机、重启场景下进行恢复。
在有复制槽的场景下，即使备库关闭很长时间，主库也会为其保留足够的 wal 日志，直到备库恢复接收完这些 wal 日志，主库才会将其删除。
当然这也带来了新的问题，即如果备库永远不恢复，那么主库的 wal 日志就会永远保留，导致磁盘空间耗尽，这时需要人工介入处理。
