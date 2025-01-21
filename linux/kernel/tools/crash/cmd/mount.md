# mount

查看设备挂载情况

```shell

crash> mount
     MOUNT           SUPERBLK     TYPE   DEVNAME   DIRNAME
ffff95ebc0030300 ffff953c40016000 rootfs rootfs    /
ffff958bc2710900 ffff958bc2661000 sysfs  sysfs     /sys
ffff958bc2712280 ffff95bbc0037000 proc   proc      /proc
ffff958bc2712880 ffff953c5904f800 devtmpfs devtmpfs /dev
ffff958bc2711080 ffff958bc0956800 securityfs securityfs /sys/kernel/security
ffff958bc2713000 ffff958bc2661800 tmpfs  tmpfs     /dev/shm
ffff958bc2711f80 ffff958bc2664000 devpts devpts    /dev/pts
ffff958bc2712100 ffff958bc2660000 tmpfs  tmpfs     /run
ffff958bc2710d80 ffff958bc2665800 tmpfs  tmpfs     /sys/fs/cgroup
ffff958bc2712700 ffff958bc2666000 cgroup cgroup    /sys/fs/cgroup/systemd
ffff958bc2710180 ffff958bc2667800 pstore pstore    /sys/fs/pstore
ffff958bc2712d00 ffff958bc2665000 efivarfs efivarfs /sys/firmware/efi/efivars
ffff958bc2710c00 ffff958bc2664800 bpf    bpf       /sys/fs/bpf
ffff958bc2711200 ffff95ebc2707000 cgroup cgroup    /sys/fs/cgroup/memory
ffff958bc2711800 ffff95ebc2701000 cgroup cgroup    /sys/fs/cgroup/cpu,cpuacct
ffff958bc2710a80 ffff95ebc2701800 cgroup cgroup    /sys/fs/cgroup/perf_event
ffff958bc2711680 ffff95ebc2704000 cgroup cgroup    /sys/fs/cgroup/files
ffff958bc2712400 ffff95ebc2700000 cgroup cgroup    /sys/fs/cgroup/freezer
ffff958bc2712e80 ffff95ebc2705800 cgroup cgroup    /sys/fs/cgroup/net_cls,net_prio
ffff958bc2710480 ffff95ebc2706000 cgroup cgroup    /sys/fs/cgroup/rdma
ffff958bc2713480 ffff95ebc2707800 cgroup cgroup    /sys/fs/cgroup/pids
ffff958bc2710600 ffff95ebc2702000 cgroup cgroup    /sys/fs/cgroup/cpuset
ffff958bc2713780 ffff95ebc2700800 cgroup cgroup    /sys/fs/cgroup/blkio
ffff958bc2710780 ffff95ebc2703800 cgroup cgroup    /sys/fs/cgroup/hugetlb
ffff958bc2710300 ffff95ebc2706800 cgroup cgroup    /sys/fs/cgroup/devices
ffff958bc236e880 ffff95ebc2753000 configfs configfs /sys/kernel/config
ffff958bd2113780 ffff95ebc35f5000 xfs    /dev/mapper/rootvg-lv_root /
ffff958bd281c300 ffff958bc266d000 rpc_pipefs rpc_pipefs /var/lib/nfs/rpc_pipefs
ffff959bc1f68000 ffff959bc39a9000 autofs systemd-1 /proc/sys/fs/binfmt_misc
ffff959bc1f6e100 ffff95ebc5f13000 hugetlbfs hugetlbfs /dev/hugepages
ffff959bc2db2a00 ffff958bc0953800 debugfs debugfs  /sys/kernel/debug
ffff959bc2db4600 ffff95bbc1f17000 mqueue mqueue    /dev/mqueue
ffff958bc236df80 ffff95ebc2753800 xfs    /dev/sda2 /boot
ffff958bce112e80 ffff95ebc2c4d800 vfat   /dev/sda1 /boot/efi
ffff958c03025800 ffff95ebd0d34800 tmpfs  tmpfs     /run/user/1005
ffff958c1440ea00 ffff95cbc28ae800 nfsd   nfsd      /proc/fs/nfsd
ffff957029adb480 ffff95ec2d6d9000 zfs    aiopool   /aiopool
ffff956cc335aa00 ffff95ec0e173800 zfs    aiopool/aio_logs /opt/aio/logs
ffff9568a2a9b900 ffff95ebc2e0e800 xfs    /dev/zd0  /volmountpoint/aiopool/1c1a5350e609_14_1735178733_panwei_datanode_34_log
ffff956b74600300 ffff955d70053000 xfs    /dev/zd16 /volmountpoint/aiopool/1c1a5350e609_14_1735178733_panwei_coordinator_6_log
ffff954f9cd43180 ffff95f02bf9e000 xfs    /dev/zd32 /volmountpoint/aiopool/1c1a5350e609_14_1735178733_panwei_gtmcoord_6_log
ffff956b74600a80 ffff955d70055800 xfs    /dev/zd48 /volmountpoint/aiopool/1c1a5350e609_14_1735178733_panwei_datanode_34_data
ffff958bf0187480 ffff9562b9e98000 xfs    /dev/zd80 /volmountpoint/aiopool/1c1a5350e609_14_1735178733_panwei_gtmcoord_6_data
ffff955bc4295380 ffff95ed8d609800 xfs    /dev/zd64 /volmountpoint/aiopool/1c1a5350e609_14_1735178733_panwei_coordinator_6_data
ffff956bd482e400 ffff95f54c64c000 tmpfs  tmpfs     /run/user/1006
```
