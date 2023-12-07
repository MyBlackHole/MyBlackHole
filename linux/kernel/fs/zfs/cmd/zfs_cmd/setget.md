# set or get


- 查询依赖关系
```shell
zfs get -r origin aiopool
```

- 指定挂载点, 启用 nfs 共享
```shell
zfs create aiopool/mountpoint_test -o mountpoint=/mnt/mountpoint_test_dir
zfs set sharenfs='rw=*' aiopool/mountpoint_test

exportfs -v
/mnt/mountpoint_test_dir <world>(sync,wdelay,hide,no_subtree_check,mountpoint,sec=sys,rw,secure,root_squash,no_all_squash)
```
