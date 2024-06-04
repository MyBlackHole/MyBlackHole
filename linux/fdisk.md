#  fdisk
分区工具

## 使用
- 查看所有磁盘信息
```shell
fdisk -l
Disk /dev/nvme0n1: 476.94 GiB, 512110190592 bytes, 1000215216 sectors
Disk model: SAMSUNG MZVLB512HBJQ-000L2
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 6851FD54-69D2-42CC-9BCA-1F309CF8A9BB

Device           Start        End   Sectors   Size Type
/dev/nvme0n1p1    2048    2099199   2097152     1G EFI System
/dev/nvme0n1p2 2099200 1000214527 998115328 475.9G Linux filesystem


Disk /dev/sda: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: SSD 860 QVO 1TB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 33553920 bytes
Disklabel type: gpt
Disk identifier: 779B54CD-AB43-4243-8D65-45BA32FA0836

Device     Start        End    Sectors   Size Type
/dev/sda1   2048 1953467279 1953465232 931.5G Linux filesystem


Disk /dev/sdb: 28.65 GiB, 30765219840 bytes, 60088320 sectors
Disk model: Ultra USB 3.0
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
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

- 扩展分区
```shell
fdisk /dev/sdb
n
p
+500M
t
1
w
```

- 格式化分区
```shell
mkfs.ext4 /dev/sdb1
```

<!-- - 清除分区 -->
<!-- ```shell -->
<!-- dd if=/dev/zero of=/dev/sdb1 bs=1M count=100 -->
<!-- ``` -->
