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

- [ 1489.223669] XFS (sdc): Superblock has unknown read-only compatible features (0x8) enabled.
[  100.734760] XFS (sdc): Superblock has unknown read-only compatible features (0x8) enabled.
[  100.738030] XFS (sdc): Attempted to mount read-only compatible filesystem read-write.
[  100.740897] XFS (sdc): Filesystem can only be safely mounted read only.
mount: mounting /dev/sdc on /xfs_test failed: Invalid argument
```shell
<!-- FS 默认格式化的时候, 会为文件系统设置一个随机的UUID, 类似 c1b9d5a2-f162-11cf-9ece-0020afc76f16,  -->
<!-- 这个uuid也可以设置为 nil 意味着文件系统UUID设置成没有UUID。 -->
<!-- 但是, 对于旧版本内核, 在激活了CRC的文件系统中, 这会导致不兼容而无法挂载文件系统。 -->
<!-- xfs内核不兼容处理 -->
<!-- 创建兼容旧版内核(3.16之前)的XFS v4 superblock -->
mkfs.xfs -i size=512 -m crc=0,finobt=0 -f /dev/sdc
```
