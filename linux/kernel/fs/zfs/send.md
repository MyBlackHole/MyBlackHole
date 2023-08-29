# send

```shell
NAME                    ORIGIN
pool/a                  -
pool/a/1                -
pool/a/1@clone          -
pool/b                  -
pool/b/1                pool/a/1@clone
pool/b/1@clone2         -
pool/b/2                pool/b/1@clone2
pool/b@pre-send         -
pool/b/1@pre-send       -
pool/b/2@pre-send       -
pool/b@send             -
pool/b/1@send           -
pool/b/2@send           -


zfs send -R pool/b@send ....

<!-- 包含以下完整流和增量流 -->
TYPE    SNAPSHOT                INCREMENTAL FROM
full    pool/b@pre-send         -
incr    pool/b@send             pool/b@pre-send
incr    pool/b/1@clone2         pool/a/1@clone
incr    pool/b/1@pre-send       pool/b/1@clone2
incr    pool/b/1@send           pool/b/1@send
incr    pool/b/2@pre-send       pool/b/1@clone2
incr    pool/b/2@send           pool/b/2@pre-send


zfs send -r pool/b@send ...

<!-- 包含以下完整流和增量流 -->
TYPE    SNAPSHOT                INCREMENTAL FROM
full    pool/b@send             -
incr    pool/b/1@clone2         pool/a/1@clone
incr    pool/b/1@send           pool/b/1@clone2
incr    pool/b/2@send           pool/b/1@clone2

zfs send -rc pool/b@send ...

<!-- 包含以下完整流和增量流 -->
TYPE    SNAPSHOT                INCREMENTAL FROM
full    pool/b@send             -
full    pool/b/1@clone2
incr    pool/b/1@send           pool/b/1@clone2
incr    pool/b/2@send           pool/b/1@clone2
```


# 例子
- 同一系统的另一个池中或用于存储备份数据的不同系统上的另一个池中接收快照流
```shell
zfs send tank/dana@snap1 | zfs recv spool/ds01
```

- 增量发送
```shell
 zfs send -i tank/dana@snap1 tank/dana@snap2 | ssh sys2 zfs recv newtank/dana
```
