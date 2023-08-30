# 回滚快照

```shell
[root@node2 ~]# zfs list -t snapshot aiopool/bookmark
NAME                             USED  AVAIL     REFER  MOUNTPOINT
aiopool/bookmark@1690442862614   171M      -      232M  -
aiopool/bookmark@1690534793819     0B      -      360M  -

<!-- 删除快照 -->
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
```
