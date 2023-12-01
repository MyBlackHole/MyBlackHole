# xfs_metadump

备份分区元数据
[[xfs_repair]] 执行前执行保底使用
xfs_metadump 仅仅是元数据，并不包含文件的真实数据

```shell
<!-- 使用 xfs_db 命令模拟文件系统损坏 -->
xfs_db -x -c blockget -c "blocktrash -s 512109 -n 10" /dev/sdb1

<!-- 使用 xfs_metadump 命令将已损坏的文件系统元数据导出 -->
xfs_metadump -o /dev/sdb1 /tmp/sdb1.metadump

<!-- 使用 xfs_mdrestore 命令将元数据转换为映像文件 -->
xfs_mdrestore /tmp/sdb1.metadump /tmp/sdb1.img

file /tmp/sdb1.img
<!-- /tmp/sdb1.img: SGI XFS filesystem data (blksz 4096, inosz 512, v2 dirs) -->

<!-- 使用 xfs_repair 命令对映像进行修复(注意不是强制修复, 没有 -L) -->
xfs_repair /tmp/sdb1.img 
```
