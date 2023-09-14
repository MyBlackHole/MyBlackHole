- 异常类型
```shell
stderr:
xfs_growfs: XFS_IOC_FSGROWFSDATA xfsctl failed: Structure needs cleaning

stderr:
data size 11534336 too small, old size is 12582912

stderr:
xfs_growfs: /volmountpoint/aiopool/192.168.90.24_88_single_log is not a mounted XFS filesystem
```

- Please unmount and run xfs_repair (>= v4.3) to resolve.
```shell
xfs_repair -f /dev/sdg -L
```
