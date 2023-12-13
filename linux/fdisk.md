#  fdisk
分区工具

## 使用
- 查看所有磁盘信息
```shell
fdisk -l
```

- 创建文件系统
```shell
sudo fdisk /dev/sdb

欢迎使用 fdisk (util-linux 2.38.1)。
更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

设备不包含可识别的分区表。
Created a new DOS (MBR) disklabel with disk identifier 0x7391b3b7.

命令(输入 m 获取帮助)：
p
n
w
```

- 删除分区信息
```shell
fdisk /dev/sdb
d (这个会默认选择一个分区)
w
```
