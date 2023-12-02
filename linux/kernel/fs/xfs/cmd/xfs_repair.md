# xfs_repair

检测修复文件系统工具
-L: 会忽略 xfs log 文件系统恢复，会导致数据缺失

- 强行恢复
```shell
xfs_repair -f /dev/sdg -L
```
