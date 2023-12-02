# send

zfs send [-DnPpRveL] [-[iI] snapshot] snapshot

必须跟随接收目标最后快照, 数据集有修改必须回滚

## 选项

### i

-i xxx1 xxx5: 发送增量, 中间快照将会合并成 xxx5
生成从第一个快照（增量源）到第二个快照（增量目标）的增量流。 增量源可以指定为
快照名称（@ 字符及其后的字符），并且假定它与增量目标来自同一文件系统。
如果目标是克隆，则源可能是原始快照，必须完全指定该快照（例如，pool/fs@origin，而不仅仅是@origin）。
```shell
<!-- 查询快照现状 -->
[root@node2 ~]# zfs list -t snapshot aiopool/4089108a1417_20_41_1690442448_goldendb_data
NAME                                                                USED  AVAIL     REFER  MOUNTPOINT
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690442862614   380K      -      232M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690443583222   175M      -      464M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690533010650   175M      -      351M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690533271931  3.22M      -      360M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690534793819  1.91M      -      360M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690768835917  66.8M      -      360M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690785527025   175M      -      515M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690788160769  60.9M      -      472M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690795031614  66.8M      -      478M  -
aiopool/4089108a1417_20_41_1690442448_goldendb_data@1691392269620   328K      -      633M  -

<!-- 发送到本地数据集 -->
[root@node2 ~]# zfs send aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690442862614 | zfs recv aiopool/bookmark

<!-- 查询发送的一个快照 -->
[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark@1690442862614     0B      -      232M  -

<!-- 发送增量 1690442862614-1690534793819 合并成 1690534793819 -->
[root@node2 ~]# zfs send -i aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690442862614 aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690534793819 | zfs recv aiopool/bookmark

[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark@1690442862614   171M      -      232M  -
aiopool/bookmark@1690534793819     0B      -      360M  -
```

### I
-I xxx1 xxx5: 发送增量, 中间快照会保留

生成一个流包，用于发送从第一个快照到第二个快照的所有中间快照。 例如，-I @a fs@d 与 -i @a fs@b 类似； -i @b fs@c; -i @c fs@d。 增量
可以使用 -i 选项指定心理来源。

```shell
[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark@1690442862614     0B      -      232M  -


<!-- 发送增量 (1690442862614,1690534793819] 之间所有增量 -->
[root@node2 ~]# zfs send -I aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690442862614 aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690534793819 | zfs recv aiopool/bookmark
您在 /var/spool/mail/root 中有邮件
[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark@1690442862614   380K      -      232M  -
aiopool/bookmark@1690443583222   175M      -      464M  -
aiopool/bookmark@1690533010650   175M      -      351M  -
aiopool/bookmark@1690533271931  3.22M      -      360M  -
aiopool/bookmark@1690534793819     0B      -      360M  -
```

### R
生成一个复制流包，它将复制指定的文件系统和所有后代文件系统，直到指定的快照。 收到后，所有属性、快照、后代
保留文件系统和克隆。
如果 -i 或 -I 标志与 -R 标志结合使用，则会生成增量复制流。 设置属性的当前值以及当前快照和文件系统名称
当接收到流时。 如果在接收该流时指定了 -F 标志，则发送端不存在的快照和文件系统将被销毁。

-D
    生成重复数据删除流。 在发送流中已发送多次的块将仅发送一次。 接收系统还必须支持此功能才能接收重复数据删除
    固定流。 无论数据集的 dedup 属性如何，都可以使用此标志，但如果文件系统使用支持 dedup 的校验和（例如 sha256），性能会好得多。

-L
    生成可能包含大于 128KB 的块的流。 如果禁用了large_blocks池功能，或者该文件系统的recordsize属性从未被设置过，则该标志无效。
    设置为128KB以上。 接收系统还必须启用large_blocks池功能。 有关 ZFS 功能标志和 large_blocks 功能的详细信息，请参阅 zpool-features(5)。

-e
    通过对通过嵌入数据池功能更紧凑地存储在磁盘上的块使用 WRITE_EMBEDDED 记录来生成更紧凑的流。 如果embedded_data
    功能被禁用。 接收系统必须启用嵌入数据功能。 如果 lz4_compress 功能在发送系统上处于活动状态，则接收系统必须具有该功能
    也启用。 有关 ZFS 功能标志和嵌入数据功能的详细信息，请参阅 zpool-features(5)。

-p
    将数据集的属性包含在流中。 当指定 -R 时，该标志是隐式的。 接收系统也必须支持此功能。

-n
    进行空运行（“No-op”）发送。 不生成任何实际发送数据。 这与 -v 或 -P 标志结合使用来确定将发送哪些数据非常有用。 在这种情况下，详细输出将
    写入标准输出（与非空运行相反，其中流写入标准输出，详细输出进入标准错误）。

-P
    打印有关生成的流包的机器可解析的详细信息。

-v
    打印有关生成的流包的详细信息。 此信息包括每秒发送的数据量的报告。


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
zfs send xxxxxxx/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@1690442862614 | zfs recv xxxxxxxx/bookmark
```

- 增量发送
```shell
zfs send -i tank/dana@snap1 tank/dana@snap2 | ssh sys2 zfs recv newtank/dana
```
