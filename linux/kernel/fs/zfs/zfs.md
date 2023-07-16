# zfs (动态文件系统)
![[imgs/img_v2_df73041f-8561-466d-a96e-9c680c24987g.webp]]

## 概念
- ZFS [[storage_pool]] 存储池
- ZFS [[dataset]] 数据集
- 虚拟设备（virtual devices,vdev）
对于操作系统来说，VDEV 和传统的 RAID 阵列卡所呈现的raid设备类似
VDEV有几种不同的类型，每种类型都有自己的优势，包括冗余和速度
一般来说，VDEV的可靠性和安全性比阵列卡要好。因此使用ZFS时不建议使用阵列卡
让ZFS直接管理磁盘。

VDEV的类型
* stripe (条带。单个磁盘，没有冗余)
* mirror (镜像。支持n-way镜像)
* raidz
	* raidz1 (一个奇偶校验磁盘, 类似于RAID 5)
	* raidz2 (两个奇偶校验磁盘, 类似于RAID 6)
	* raidz3 (三个奇偶校验磁盘, 没有类似RAID等级)
* disk（磁盘）
* file (文件。不推荐在生产环境中使用，因为中间又多了一层不必要的文件系统)

数据会以条带方式存储于存储池中的所有VDEV上。因此一个存储池中的VDEV越多，IOPS就越高。

## build
```shell
# 需要关闭 python 虚拟环境
./autogen.sh
./configure CFLAGS='-g -O0' (./configure --enable-debug)
# ./configure CFLAGS='-g -O0' --prefix=/media/black/Data/lib/zfs/master
make all
```

## 安装
```shell
#centos
# 现在安装内核开发和zfs包
# 内核开发的软件包是需要ZFS建立模块和插入到内核中
yum install epel-release
yum install kernel-devel zfs

# ubuntu
sudo apt install zfsutils-linux
```


## 例子

- 查询
```shell
zfs list -t snapshot -o name,volsize,used,refer,avail
```
