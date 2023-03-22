# zfs (动态文件系统)

## 概念
### 虚拟设备（virtual devices,vdev）
对于操作系统来说，VDEV和传统的RAID阵列卡所呈现的raid设备类似。VDEV有几种不同的类型，每种类型
都有自己的优势，包括冗余和速度。一般来说，VDEV的可靠性和安全性比阵列卡要好。因此使用ZFS时不
建议使用阵列卡。让ZFS直接管理磁盘。

VDEV的类型
* stripe (条带。单个磁盘，没有冗余)
* mirror (镜像。支持n-way镜像)
* raidz
	* raidz1 (一个奇偶校验磁盘, 类似于RAID 5)
	* raidz2 (两个奇偶校验磁盘, 类似于RAID 6)
	* raidz3 (三个奇偶校验磁盘, 没有类似RAID等级)
* disk  （磁盘）
* file (文件。不推荐在生产环境中使用，因为中间又多了一层不必要的文件系统)

数据会以条带方式存储于存储池中的所有VDEV上。因此一个存储池中的VDEV越多，IOPS就越高。

### storage pool （存储池） 

ZFS 使用存储池来作为底层存储提供者（VDEV）的抽象。这样可以把用户可见的文件系统和底层的物理磁盘
布局分离开来。

### ZFS 数据集（Dataset）

ZFS 数据集类似于传统的文件系统（译者注：或者说是目录），但是提供了更多的功能。ZFS的很多优势也是
在这一层体现出来的。数据集支持 [Copy on Write](https://en.wikipedia.org/wiki/Copy-on-write)
快照, 配额, 压缩和重复消除（de-duplication）.

### 限制

一个目录最多可包含 2^48个文件, 每个文件最大可以是16 exabytes.  一个存储池最大可包含256 zettabytes 、
(2^78) 的空间, 可以条带化地分布于2^64 设备上. 单一主机最多可以创建2^64个存储池。这些限制可以说是相
当大。

## 命令

### 存储池

Actions: （存储池操作） 
* List   （列举）
* Status （查看状态）
* Destroy （删除）
* Get/Set properties （获取/设置属性）

List zpools （列举存储池（也叫zpool））

```bash
# 创建一个raidz类型的存储池(名称为bucket）
$ zpool create bucket raidz1 gpt/zfs0 gpt/zfs1 gpt/zfs2
# 列出所有存储池
$ zpool list
NAME    SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
zroot   141G   106G  35.2G         -    43%    75%  1.00x  ONLINE  -
# 列出某一存储池的详细信息
$ zpool list -v zroot
NAME                                     SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP HEALTH  ALTROOT
zroot                                    141G   106G  35.2G         -    43%    75%  1.00x ONLINE  -
  gptid/c92a5ccf-a5bb-11e4-a77d-001b2172c655   141G   106G  35.2G         -    43%    75%
```

Status of zpools （存储池状态）

```bash
# 获取全部zpool状态信息
$ zpool status
  pool: zroot
 state: ONLINE
  scan: scrub repaired 0 in 2h51m with 0 errors on Thu Oct  1 07:08:31 2015
config:
        NAME                                          STATE     READ WRITE CKSUM
        zroot                                         ONLINE       0     0     0
          gptid/c92a5ccf-a5bb-11e4-a77d-001b2172c655  ONLINE       0     0     0
errors: No known data errors
# 用scrub来更正存储池错误信息
$ zpool scrub zroot
$ zpool status -v zroot
  pool: zroot
 state: ONLINE
  scan: scrub in progress since Thu Oct 15 16:59:14 2015
        39.1M scanned out of 106G at 1.45M/s, 20h47m to go
        0 repaired, 0.04% done
config:
        NAME                                          STATE     READ WRITE CKSUM
        zroot                                         ONLINE       0     0     0
          gptid/c92a5ccf-a5bb-11e4-a77d-001b2172c655  ONLINE       0     0     0
errors: No known data errors
```

Properties of zpools （存储池属性）

```bash
# 获取某一存储池的全部属性。属性可能是系统提供，也可能是用户设置
$ zpool get all zroot
NAME   PROPERTY                       VALUE                          SOURCE
zroot  size                           141G                           -
zroot  capacity                       75%                            -
zroot  altroot                        -                              default
zroot  health                         ONLINE                         -
...
# 设置存储池属性，下例这是设置comment(备注)属性
$ zpool set comment="Storage of mah stuff" zroot
$ zpool get comment
NAME   PROPERTY  VALUE                 SOURCE
tank   comment   -                     default
zroot  comment   Storage of mah stuff  local
```

Remove zpool （删除存储池）

```bash
$ zpool destroy test
```

### Datasets （数据集）

Actions:   （数据集相关操作）
* Create   （创建）
* List     （列举）
* Rename   （重命名）
* Delete   （删除）
* Get/Set properties   （获取/设置属性）

Create datasets

```bash
# 创建数据集
$ zfs create tank/root/data
$ mount | grep data
tank/root/data on /data (zfs, local, nfsv4acls)
# 创建子数据集
$ zfs create tank/root/data/stuff
$ mount | grep data
tank/root/data on /data (zfs, local, nfsv4acls)
tank/root/data/stuff on /data/stuff (zfs, local, nfsv4acls)
# 创建卷
$ zfs create -V zroot/win_vm
$ zfs list zroot/win_vm
NAME                 USED  AVAIL  REFER  MOUNTPOINT
tank/win_vm         4.13G  17.9G    64K  -
```

List datasets （列举数据集）

```bash
# 列出所有数据集
$ zfs list
NAME                                                                       USED  AVAIL  REFER  MOUNTPOINT
zroot                                                                      106G  30.8G   144K  none
zroot/ROOT                                                                18.5G  30.8G   144K  none
zroot/ROOT/10.1                                                              8K  30.8G  9.63G  /
zroot/ROOT/default                                                        18.5G  30.8G  11.2G  /
zroot/backup                                                              5.23G  30.8G   144K  none
zroot/home                                                                 288K  30.8G   144K  none
...
# 列举某一数据集的信息
$ zfs list zroot/home
NAME         USED  AVAIL  REFER  MOUNTPOINT
zroot/home   288K  30.8G   144K  none
# 列出快照
$ zfs list -t snapshot
zroot@daily-2015-10-15                                                                  0      -   144K  -
zroot/ROOT@daily-2015-10-15                                                             0      -   144K  -
zroot/ROOT/default@daily-2015-10-15                                                     0      -  24.2G  -
zroot/tmp@daily-2015-10-15                                                           124K      -   708M  -
zroot/usr@daily-2015-10-15                                                              0      -   144K  -
zroot/home@daily-2015-10-15                                                             0      -  11.9G  -
zroot/var@daily-2015-10-15                                                           704K      -  1.42G  -
zroot/var/log@daily-2015-10-15                                                       192K      -   828K  -
zroot/var/tmp@daily-2015-10-15                                                          0      -   152K  -
```

Rename datasets （重命名数据集）

```bash
$ zfs rename tank/root/home tank/root/old_home
$ zfs rename tank/root/new_home tank/root/home
```

Delete dataset （删除数据集）

```bash
# 数据集如果有快照则无法删除
zfs destroy tank/root/home
```

Get / set properties of a dataset （获取/设置数据集属性）

```bash
# 获取数据集全部属性
$ zfs get all  zroot/usr/home                                                                                              │157 # Create Volume
NAME            PROPERTY              VALUE                  SOURCE                                                                          │158 $ zfs create -V zroot/win_vm
zroot/home      type                  filesystem             -                                                                               │159 $ zfs list zroot/win_vm
zroot/home      creation              Mon Oct 20 14:44 2014  -                                                                               │160 NAME                 USED  AVAIL  REFER  MOUNTPOINT
zroot/home      used                  11.9G                  -                                                                               │161 tank/win_vm         4.13G  17.9G    64K  -
zroot/home      available             94.1G                  -                                                                               │162 ```
zroot/home      referenced            11.9G                  -                                                                               │163
zroot/home      mounted               yes                    -
...
# 获取数据集属性
$ zfs get compression zroot/usr/home
NAME            PROPERTY     VALUE     SOURCE
zroot/home      compression  off       default
# 设置数据集属性（下例为设置压缩属性compression）
$ zfs set compression=gzip-9 mypool/lamb
# 列举所有数据集的名称、配额和预留属性
$ zfs list -o name,quota,reservation
NAME                                                               QUOTA  RESERV
zroot                                                               none    none
zroot/ROOT                                                          none    none
zroot/ROOT/default                                                  none    none
zroot/tmp                                                           none    none
zroot/usr                                                           none    none
zroot/home                                                          none    none
zroot/var                                                           none    none
...
```
### Snapshots （快照）
快照是ZFS 的一个非常重要的功能
* 快照占用的空间等于它和原始数据的差异量
* 创建时间以秒计
* 恢复时间和写入速度相同
* 易于自动化
Actions:  （快照相关操作）
* Create  （创建）
* Delete  （删除）
* Rename  （重命名）
* Access snapshots  （访问）
* Send / Receive    （发送/接收）
* Clone             （克隆。译者注：关于clone和快照的区别可参看[这里](http://docs.oracle.com/cd/E19253-01/819-5461/gbcxz/index.html)）
Create snapshots （创建快照）
```bash
# 为单一数据集创建快照
zfs snapshot tank/home/sarlalian@now
# 为数据集及其子集创建快照
$ zfs snapshot -r tank/home@now
$ zfs list -t snapshot
NAME                       USED  AVAIL  REFER  MOUNTPOINT
tank/home@now                 0      -    26K  -
tank/home/sarlalian@now       0      -   259M  -
tank/home/alice@now           0      -   156M  -
tank/home/bob@now             0      -   156M  -
...
Destroy snapshots （删除快照）
```bash
# 如何删除快照
$ zfs destroy tank/home/sarlalian@now
# 删除某一数据集及其子集的快照
$ zfs destroy -r tank/home/sarlalian@now
```

Renaming Snapshots （重命名）

```bash
# 重命名快照
$ zfs rename tank/home/sarlalian@now tank/home/sarlalian@today
$ zfs rename tank/home/sarlalian@now today
# zfs rename -r tank/home@now @yesterday
```

Accessing snapshots  （访问快照）

```bash
# cd进入一个快照目录
$ cd /home/.zfs/snapshot/
```

Sending and Receiving

```bash
# 备份快照到一个文件
$ zfs send tank/home/sarlalian@now | gzip > backup_file.gz
# 发送快照到另一个数据集
$ zfs send tank/home/sarlalian@now | zfs recv backups/home/sarlalian
# 发送快照到一个远程主机
$ zfs send tank/home/sarlalian@now | ssh root@backup_server 'zfs recv tank/home/sarlalian'
# 发送完整数据集及其快照到一个新主机
$ zfs send -v -R tank/home@now | ssh root@backup_server 'zfs recv tank/home'
```

Cloneing Snapshots  （克隆快照）

```bash
# 克隆一个快照
$ zfs clone tank/home/sarlalian@now tank/home/sarlalian_new
# 提升克隆，让它不再依赖原始快照
$ zfs promote tank/home/sarlalian_new
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

## 使用
fdisk[[fdisk]] 分区工具
使用详情[https://www.escapelife.site/posts/caf259ea.html#toc-heading-9]
- 创建池
```shell
zpool create -f aiopool sdb # 会自动挂载到根目录 aiopool 目录

# RAID0 (数据条带化后，支持并行访问，提高读取速度)
zpool create -f aiopool sdb sda

# RAIO1 (镜像,用一半空间用于复制)
zpool create -f aiopool mirror sdb sda

# RAIO5/RAIOZ1 (驱动器至少是3个)
zpool create -f aiopool raidz1 sdb sda sdc

# RAID6/RAIDZ2 (驱动器至少是4个)
zpool create -f aiopool raidz2 sdb sda sdc sdd

# RAID10 (条带化镜像)
zpool create -f aiopool mirror sdb sda mirror sdc sdd
```

- 销毁池
```shell
zpool destroy aiopool
```

- 查询池状态
```shell
zpool status
zpool status -x
zpool list
zpool get all

# 对应信息解释
$ sudo zpool status
  pool: aiopool   # 池的名称
 state: ONLINE  # 池的当前运行状况
  scan: scrub repaired 0 in 0h39m with 0 errors on Sun Dec  8 01:03:48 2023

config: # 发出读取请求时出现I/O错误|发出写入请求时出现I/O错误|校验和错误
  NAME        STATE     READ WRITE CKSUM
aiopool     ONLINE       0     0     0
  sdb       ONLINE       0     0     0

errors: No known data errors  # 确定是否存在已知的数据错误
```

- 池修改历史
```shell
zpool history
```

- 更新指定池
```shell
zpool upgrade aiopool
```

- 更新所有池
```shell
zpool upgrade -a
```

- 添加硬盘
```shell
zpool add aiopool -f sda
```

- 删除硬盘
```shell
zpool remove aiopool sda
```

- 查看正在使用的文件系统
```shell
zfs list

NAME      USED  AVAIL     REFER  MOUNTPOINT
aiopool   116K  48.0G       24K  /aiopool
```

- 数据集创建
```shell
zfs create aiopool/home
```

- 快照创建
```shell
zfs snapshot aiopool@test1
```

- 快照容量
```shell
zfs list -t snapshot

NAME            USED  AVAIL     REFER  MOUNTPOINT
aiopool@test1    13K      -       24K  -
```
