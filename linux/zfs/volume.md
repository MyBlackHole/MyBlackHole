# volume

## 例子
- 1G大小 创建卷
```shell
# 创建卷
zfs create -V 20G aiopool/iscsi_test_dev

zfs list aiopool/iscsi_test_dev
NAME                     USED  AVAIL     REFER  MOUNTPOINT
aiopool/iscsi_test_dev  20.6G   144G       56K  -


ls -alh /dev/aiopool/
总用量 0
drwxr-xr-x  2 root root   60 May 26 17:15 .
drwxr-xr-x 21 root root 3.2K May 26 17:15 ..
lrwxrwxrwx  1 root root    6 May 26 17:15 iscsi_test_dev -> ../zd0
```

- 销毁卷
```shell
zfs destroy aiopool/iscsi_test_dev_3
```
