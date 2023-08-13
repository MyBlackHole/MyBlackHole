# send

- 同一系统的另一个池中或用于存储备份数据的不同系统上的另一个池中接收快照流
```shell
zfs send tank/dana@snap1 | zfs recv spool/ds01
```

- 增量发送
```shell
 zfs send -i tank/dana@snap1 tank/dana@snap2 | ssh sys2 zfs recv newtank/dana
```
