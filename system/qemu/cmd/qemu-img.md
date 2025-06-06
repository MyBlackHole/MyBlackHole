# qemu-img

- 格式转换
```shell
qemu-img convert -f raw -O qcow2 archlinux-x86_64.iso archlinux-x86_64.qcow2
```

- 创建虚拟磁盘
```shell
qemu-img create ubuntu-20.04.5-amd64.img 10G

qemu-img create -f raw block.img 320M

qemu-img create -f qcow2 openEuler-20.03-LTS-SP2-x86_64.qcow2 40G

qemu-img create -f qcow2 -o preallocation=metadata,cluster_size=65536 openEuler-20.03-LTS-SP2-x86_64.qcow2 40G
```

- 查看磁盘信息
```shell
❯ qemu-img info openEuler-20.03-LTS-SP2-x86_64.qcow2
image: openEuler-20.03-LTS-SP2-x86_64.qcow2
file format: qcow2
virtual size: 40 GiB (42949672960 bytes)
disk size: 1.8 GiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    compression type: zlib
    lazy refcounts: false
    refcount bits: 16
    corrupt: false
    extended l2: false
Child node '/file':
    filename: openEuler-20.03-LTS-SP2-x86_64.qcow2
    protocol type: file
    file length: 1.85 GiB (1987444736 bytes)
    disk size: 1.8 GiB
```

- 调整磁盘大小
```shell

# 只有raw格式的镜像同时支持增大和缩小,
# 而qcow2格式只支持增大。

qemu-img resize archlinux-x86_64.qcow2 +10G

# ext4
❯ qemu-img resize ./openEuler-20.03-LTS-SP2-x86_64.qcow2 +20G
Image resized.

fdisk /dev/sda
Command (m for help): p
Device     Boot   Start      End  Sectors Size Id Type
/dev/sda1          2048  4194303  4192256   2G  e W95 FAT16 (LBA)
/dev/sda2       4194304 83886079 79691776  38G 83 Linux
Command (m for help): d
Partition number (1,2, default 2): 2
Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2):
First sector (4194304-125829119, default 4194304):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4194304-125829119, default 125829119)

Created a new partition 2 of type 'Linux' and of size 58 GiB.
Partition #2 contains a ext4 signature.

Do you want to remove the signature? [Y]es/[N]o: y

The signature will be removed by a write command.

Command (m for help): w
The partition table has been altered.
Syncing disks.

[root@localhost ~]# resize2fs /dev/sda2
resize2fs 1.45.6 (20-Mar-2020)
Filesystem at /dev/sda2 is mounted on /; on-line resizing required
old_desc_blocks = 5, new_desc_blocks = 8
The filesystem on /dev/sda2 is now 15204352 (4k) blocks long.


```


- 快照
```shell
# 列出快照
qemu-img snapshot -l openEuler-20.03-LTS-SP2-x86_64.qcow2
Snapshot list:
ID      TAG               VM_SIZE                DATE        VM_CLOCK     ICOUNT
1       snap1                 0 B 2025-06-04 14:11:20  0000:00:00.000          0

# 创建快照
qemu-img snapshot -c snap1 openEuler-20.03-LTS-SP2-x86_64.qcow2

# 回滚快照
qemu-img snapshot -a snap1 openEuler-20.03-LTS-SP2-x86_64.qcow2

# 删除快照
qemu-img snapshot -d snap1 openEuler-20.03-LTS-SP2-x86_64.qcow2
```
