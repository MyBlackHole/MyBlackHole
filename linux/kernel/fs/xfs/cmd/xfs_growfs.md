# xfs_growfs
使用xfs_growfs命令增加 XFS 文件系统的大小。必须挂载 XFS 文件系统，并且底层设备上必须有可用空间

## 语法
xfs_growfs [options] mount-point

### options
-d：将文件系统的数据部分扩展到底层设备的最大大小。
-D [size] : 指定扩展文件系统数据部分的大小。[size] 参数以文件系统块的数量表示。
-L [size]：指定日志区域的新大小。这不会扩展大小，而是指定日志区域的新大小。因此，此选项可用于缩小日志区域的大小。您不能缩小文件系统的数据部分的大小。
-m [maxpct]：指定文件系统中可分配为 inode 的最大空间百分比的新值。对于 mkfs.xfs 命令，此选项是通过–i maxpct=[value]选项指定的。
