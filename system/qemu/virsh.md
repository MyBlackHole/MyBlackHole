# virsh

## 网络
```sh
# 列出所有虚拟机网络
$ sudo virsh net-list --all
 Name      State      Autostart   Persistent
----------------------------------------------
 default   inactive   no          yes

# 启动默认网络
$ virsh net-start default
Network default started

# 将 default 网络设为自启动
$ virsh net-autostart --network default
Network default marked as autostarted

# 再次检查网络状况，已经是 active 了
$ sudo virsh net-list --all
 Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes

```

## 管理虚拟机
- 创建虚拟机
```shell
# 使用 iso 镜像创建全新的 proxmox 虚拟机，自动创建一个 60G 的磁盘。
virt-install --virt-type kvm \
--name pve-1 \
--vcpus 4 --memory 8096 \
--disk size=60 \
--network network=default,model=virtio \
--os-type linux \
--os-variant generic \
--graphics vnc \
--cdrom proxmox-ve_6.3-1.iso

# 使用已存在的 opensuse cloud 磁盘创建虚拟机
virt-install --virt-type kvm \
  --name opensuse15-2 \
  --vcpus 2 --memory 2048 \
  --disk opensuse15.2-openstack.qcow2,device=disk,bus=virtio \
  --disk seed.iso,device=cdrom \
  --os-type linux \
  --os-variant opensuse15.2 \
  --network network=default,model=virtio \
  --graphics vnc \
  --import
```

- 查看虚拟机
```sh
# 查看正在运行的虚拟机
virsh list

# 查看所有虚拟机，包括 inactive 的虚拟机
virsh list --all

```


- 连接虚拟机
```sh
# 使用虚拟机 ID 连接
virt-viewer 8
# 使用虚拟机名称连接，并且等待虚拟机启动
virt-viewer --wait opensuse15

```

- 启动、关闭、暂停(休眠)、重启虚拟机
```sh
virsh start opensuse15
virsh suuspend opensuse15
virsh resume opensuse15
virsh reboot opensuse15
# 优雅关机
virsh shutdown opensuse15
# 强制关机
virsh destroy opensuse15

# 启用自动开机
virsh autostart opensuse15
# 禁用自动开机
virsh autostart --disable opensuse15

```


- 虚拟机快照管理

```sh
# 列出一个虚拟机的所有快照
virsh snapshot-list --domain opensuse15
# 给某个虚拟机生成一个新快照
virsh snapshot-create <domain>
# 使用快照将虚拟机还原
virsh snapshot-restore <domain> <snapshotname>
# 删除快照
virsh snapshot-delete <domain> <snapshotname>

```

- 删除虚拟机

```sh
`virsh undefine opensuse15 `
```

- 迁移虚拟机

```sh
# 使用默认参数进行离线迁移，将已关机的服务器迁移到另一个 qemu 实例
virsh migrate 37 qemu+ssh://tux@jupiter.example.com/system
# 还支持在线实时迁移，待续

```

- cpu/内存修改

```sh
# 改成 4 核
virsh setvcpus opensuse15 4
# 改成 4G
virsh setmem opensuse15 4096

```


- 虚拟机监控
```sh
# 查看虚拟机的 CPU、内存、网络、磁盘使用情况
virsh domstats opensuse15
# 查看虚拟机的日志
virsh domlog opensuse15-2
# 查看虚拟机的快照
virsh domsnapshot opensuse15-2
# 查看虚拟机的配置
virsh dumpxml opensuse15-2
```

- 修改磁盘、网络及其他设备

```sh
# 添加新设备
virsh attach-device
virsh attach-disk
virsh attach-interface
# 删除设备
virsh detach-disk
virsh detach-device
virsh detach-interface

```
