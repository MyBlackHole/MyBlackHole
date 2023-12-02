# xfs

[细节](https://zhuanlan.zhihu.com/p/137906421)

- 安装
```shell
sudo apt-get install xfsprogs
```

- 使用
```shell
sudo mkfs.xfs -f /dev/sda1
```

- 内核启用
```shell
make menuconfig

# * 代表编译进内核\ M 代表编译成 mod
File systems  --->
<*> XFS filesystem support                
[*]   Support deprecated V4 (crc=0) format
[ ]   XFS Quota support                   
[ ]   XFS POSIX ACL support               
[ ]   XFS Realtime subvolume support      
[ ]   XFS online metadata check support   
[ ]   XFS Verbose Warnings                
[ ]   XFS Debugging support               
```

## 命令
```bash
xfs_admin      xfs_db         xfs_freeze     xfs_info       xfs_logprint   xfs_mkfile     xfs_repair
xfs_bmap       xfsdump        xfs_fsr        xfsinvutil     xfs_mdrestore  xfs_ncheck     xfsrestore
xfs_copy       xfs_estimate   xfs_growfs     xfs_io         xfs_metadump   xfs_quota      xfs_rtcp
```
