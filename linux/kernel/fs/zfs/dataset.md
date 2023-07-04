# dataset (数据集)
ZFS 数据集类似于传统的文件系统（译者注：或者说是目录），但是提供了更多的功能。ZFS的很多优势也是
在这一层体现出来的。数据集支持 [Copy on Write](https://en.wikipedia.org/wiki/Copy-on-write)
快照, 配额, 压缩和重复消除（de-duplication）.

- 限制
一个目录最多可包含 2^48个文件, 每个文件最大可以是16 exabytes.  一个存储池最大可包含256 zettabytes 、
(2^78) 的空间, 可以条带化地分布于2^64 设备上. 单一主机最多可以创建2^64个存储池。这些限制可以说是相
当大。

## 使用例子

Actions:   （数据集相关操作）
* Create   （创建）
* List     （列举）
* Rename   （重命名）
* Delete   （删除）
* Get/Set properties   （获取/设置属性）

### Create datasets

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

```

### List datasets （列举数据集）

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

### Rename datasets （重命名数据集）

```bash
$ zfs rename tank/root/home tank/root/old_home
$ zfs rename tank/root/new_home tank/root/home
```

### Delete dataset （删除数据集）

```bash
# 数据集如果有快照则无法删除
zfs destroy tank/root/home
```

### Get / set properties of a dataset （获取/设置数据集属性）

```bash
# 获取数据集全部属性
$ zfs get all zroot/usr/home                                                                                              │157 # Create Volume
NAME            PROPERTY              VALUE                  SOURCE                                                                          │158 $ zfs create -V zroot/win_vm
zroot/home      type                  filesystem             -                                                                               │159 $ zfs list zroot/win_vm
zroot/home      creation              Mon Oct 20 14:44 2014  -                                                                               │160 NAME                 USED  AVAIL  REFER  MOUNTPOINT
zroot/home      used                  11.9G                  -                                                                               │161 tank/win_vm         4.13G  17.9G    64K  -
zroot/home      available             94.1G                  -                                                                               │162 
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

## 例子

- 查询依赖关系
```shell
zfs get -r origin aiopool
```

- 指定挂载点, 启用 nfs 共享
```shell
zfs create aiopool/mountpoint_test -o mountpoint=/mnt/mountpoint_test_dir
zfs set sharenfs='rw=*' aiopool/mountpoint_test

exportfs -v
/mnt/mountpoint_test_dir
                <world>(sync,wdelay,hide,no_subtree_check,mountpoint,sec=sys,rw,secure,root_squash,no_all_squash)
```
