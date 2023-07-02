# volume
数据卷

## 操作

### create
创建卷
zfs create [-ps] [-b blocksize] [-o property=value] … -V [size] [volume]

- 例如
```shell
zfs create -V 20G aiopool/iscsi_test_dev
# 格式化文件系统
mkfs.ext4 /dev/aiopool/iscsi_test_dev
```

### rename
重命名卷
zfs rename

- 例如
```shell
zfs rename aiopool/iscsi_test_dev1 aiopool/iscsi_test_dev2

zfs list
NAME                      USED  AVAIL     REFER  MOUNTPOINT
aiopool                  2.09G  45.9G       96K  /aiopool
aiopool/iscsi_test_dev   1.03G  46.9G      968K  -
aiopool/iscsi_test_dev2  1.03G  46.9G       56K  -
aiopool/test_write_full  22.8M  45.9G      120K  -
```

### destroy
销毁卷

- 例如
```shell
zfs destroy aiopool/iscsi_test_dev2 -f

zfs list
NAME                      USED  AVAIL     REFER  MOUNTPOINT
aiopool                  1.06G  46.9G       96K  /aiopool
aiopool/iscsi_test_dev   1.03G  47.9G      968K  -
aiopool/test_write_full  22.8M  46.9G      120K  -
```

## 例子
- 1G大小 创建卷
```shell
# 创建卷
zfs create -V 20G aiopool/iscsi_test_dev

zfs list aiopool/iscsi_test_dev
NAME                     USED  AVAIL     REFER  MOUNTPOINT
aiopool/iscsi_test_dev  20.6G   144G       56K  -

# 格式化文件系统
mkfs.ext4 /dev/aiopool/iscsi_test_dev

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

- 测试卷写满是否会异常
```shell
zfs create -V 20M aiopool/test_write_full
mkfs.ext4 /dev/aiopool/test_write_full
mount /dev/zd32 /mnt/zd32_dir/


Traceback (most recent call last):
  File "test_write.py", line 9, in <module>
    f.flush()
OSError: [Errno 28] No space left on device
```

- 测试写途中修改卷为只读
```shell
zfs create -V 20M aiopool/test_write_full
mkfs.ext4 /dev/aiopool/test_write_full
mount /dev/zd32 /mnt/zd32_dir/
```
