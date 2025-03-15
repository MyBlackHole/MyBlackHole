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

- 
[6362370.959645] XFS (loop0): Superblock has unknown read-only compatible features (0x4) enabled.
[6362370.961015] XFS (loop0): Attempted to mount read-only compatible filesystem read-write.
[6362370.961017] XFS (loop0): Filesystem can only be safely mounted read only.
[6362370.961434] XFS (loop0): SB validate failed with error -22.

+ mount -o nouuid /testmount/test-bc-block/aiopool/10.6.66.123_492_single/1740713162633 /opt/aio/data
mount: wrong fs type, bad option, bad superblock on /dev/loop0,
       missing codepage or helper program, or other error

       In some cases useful info is found in syslog - try
       dmesg | tail or so.
