# ceph


## 例子
- Rook Ceph OSD异常，格式化osd硬盘重新挂载
```shell
<!-- 命名用ceph命令查询并删除osd -->

<!-- 查询状态，找到要移除的osd id -->
ceph osd status
<!-- 标记移除的 osd -->
ceph osd out osd.1
ceph osd purge 1 --yes-i-really-mean-it
ceph osd crush remove osd.1
ceph auth rm osd.1
ceph osd rm osd.1

<!-- 删除相关 osd 节点的 deployment -->
kubectl delete deploy rook-ceph-osd-1 -n rook-ceph

<!-- 登录要删除 osd 所在的服务器，格式化 osd 硬盘 -->
<!-- 检查硬盘路径 -->
fdisk -l
<!-- 删除硬盘分区信息 -->
DISK="/dev/sdb"
sgdisk --zap-all $DISK
<!-- 清理硬盘数据（hdd硬盘使用dd，ssd硬盘使用blkdiscard，二选一） -->
dd if=/dev/zero of="$DISK" bs=1M count=100 oflag=direct,dsync
blkdiscard $DISK
<!-- 删除原 osd 的 lvm 信息（如果单个节点有多个 osd，那么就不能用*拼配模糊删除，而根据lsblk -f查询出明确的lv映射信息再具体删除，参照第5项操作） -->
ls /dev/mapper/ceph-* | xargs -I% -- dmsetup remove %
rm -rf /dev/ceph-*
<!-- 重启，sgdisk –zzap-all需要重启后才生效 -->
reboot


#查看lvm设备信息
dmsetup ls;
#删除ceph osd lvm映射关系
dmsetup remove ceph--5a4cb4bb--70b3--40bd--9da7--09d4f264a513-osd-xxxxxxxxx
#移动lv
lvremove /dev/mapper/ceph--5a4cb4bb--70b3--40bd--9da7--09d4f264a513-osd—xxxxxxxxx
#删除相关文件
rm –rf /dev/ceph--5a4cb4bb--70b3--40bd--9da7--09d4f264a513


<!-- 重启 ceph operator 调度，使检测到格式化后的 osd 硬盘，osd 启动后 ceph 集群会自动平衡数据 -->
kubectl rollout restart deploy rook-ceph-operator -n rook-ceph

<!-- 注：如果新osd pod无法执行起来可以通过查询osd prepare日志找问题 -->
kubectl -n rook-ceph logs rook-ceph-osd-prepare-node1-fvmrp provision
```
