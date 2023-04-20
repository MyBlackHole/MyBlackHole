# Snapshots (快照)

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

## Create snapshots （创建快照）
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

## Renaming Snapshots （重命名）

```bash
# 重命名快照
$ zfs rename tank/home/sarlalian@now tank/home/sarlalian@today
$ zfs rename tank/home/sarlalian@now today
# zfs rename -r tank/home@now @yesterday
```

## Accessing snapshots  （访问快照）

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

## Cloneing Snapshots  （克隆快照）

```bash
# 克隆一个快照
$ zfs clone tank/home/sarlalian@now tank/home/sarlalian_new
# 提升克隆，让它不再依赖原始快照
$ zfs promote tank/home/sarlalian_new
```

```shell
zfs destroy -r aiopool/192.168.78.213_6_file
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
