# volume

## 例子
- 1G大小 创建卷
```shell
zfs clone aiopool/iscsi_test_dev
zfs list aiopool/iscsi_test_dev
NAME                     USED  AVAIL     REFER  MOUNTPOINT
aiopool/iscsi_test_dev  1.03G  48.0G       56K  -


[root@node2 aiopool]# ls -alh /dev/aiopool/
总用量 0
drwxr-xr-x  2 root root   60 May 26 17:15 .
drwxr-xr-x 21 root root 3.2K May 26 17:15 ..
lrwxrwxrwx  1 root root    6 May 26 17:15 iscsi_test_dev -> ../zd0
```
