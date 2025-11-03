# bookmark
标签,替代**发送流**快照存在,为了增量发送留埋点
空间占用小(记录 guid\createtxg)
createtxg 全局唯一永久递增

```shell
root@black:/sandisk# zfs get all sandisk#1
NAME       PROPERTY              VALUE                   SOURCE
sandisk#1  type                  bookmark                -
sandisk#1  creation              日 8月 13 18:25 2023  -
sandisk#1  referenced            155M                    -
sandisk#1  createtxg             7242                    -
sandisk#1  guid                  6320954441941772454     - (全局唯一标识)
sandisk#1  logicalreferenced     155M                    -  (逻辑产考大小)
```

- 显示所有标签
```shell
root@black:/sandisk# sudo zfs list -t bookmark
NAME        USED  AVAIL     REFER  MOUNTPOINT
sandisk#1      -      -      155M  -
```

- 给快照生成标签
```shell
zfs bookmark sandisk@1 sandisk#1
```

- 删除标签
```shell
zfs destroy sandisk#1
```

- bookmark 使用案例
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

<!-- 删除，测试其它发送场景 -->
zfs destroy aiopool/bookmark@1690534793819

<!-- 当前文件系统已经在快照 1690534793819 上 -->
<!-- 增量无法发送, 需要回滚 -->
[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark@1690442862614   171M      -      232M  -

<!-- 需要回滚 -->
[root@node2 ~]# zfs rollback aiopool/bookmark@1690442862614
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

<!-- 删除中间快照，发送 1690533010650 增量测试 -->
[root@node2 ~]# zfs destroy aiopool/bookmark@1690533010650


<!-- 发送 1690533010650 增量测试失败 -->
[root@node2 ~]# zfs send -I aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690442862614 aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690533010650 | zfs recv aiopool/bookmark
cannot receive incremental stream: destination aiopool/bookmark has been modified
since most recent snapshot


[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark@1690442862614   380K      -      232M  -
aiopool/bookmark@1690443583222   223M      -      464M  -
aiopool/bookmark@1690533271931  3.23M      -      360M  -
aiopool/bookmark@1690534793819     0B      -      360M  -

<!-- 清除快照 -->
[root@node2 ~]# zfs bookmark aiopool/bookmark@1690442862614 aiopool/bookmark#1690442862614
[root@node2 ~]# zfs destroy aiopool/bookmark@1690534793819
[root@node2 ~]# zfs destroy aiopool/bookmark@1690533271931
[root@node2 ~]# zfs destroy aiopool/bookmark@1690443583222
[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark@1690442862614   171M      -      232M  -
[root@node2 ~]# zfs destroy aiopool/bookmark@1690442862614
[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
no datasets available
[root@node2 ~]# zfs list -t bookmark aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark#1690442862614      -      -         -  -


<!-- 由于全部快照已清除， aiopool/bookmark 最新快照没有匹配增量流 -->
[root@node2 ~]# zfs send -I aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690442862614 aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690533010650 | zfs recv aiopool/bookmark
cannot receive incremental stream: most recent snapshot of aiopool/bookmark does not
match incremental source



```
