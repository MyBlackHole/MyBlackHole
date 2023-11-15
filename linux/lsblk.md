# lsblk
用于列出所有可用块设备

```shell
yum install util-linux
```

## 选项
默认不会显示空设备 
-a, --all            显示所有设备。
-b, --bytes          以bytes方式显示设备大小。
-d, --nodeps         不显示 slaves 或 holders。
-D, --discard        print discard capabilities。
-e, --exclude <list> 排除设备 (default: RAM disks)。
-I, --include <列表> 只显示有指定主设备号的设备
-f, --fs             显示文件系统信息。
-h, --help           显示帮助信息。
-i, --ascii          use ascii characters only。
-m, --perms          显示权限信息。
-l, --list           使用列表格式显示。
-n, --noheadings     不显示标题。
-o, --output <list>  输出列。
-P, --pairs          使用key="value"格式显示。
-r, --raw            使用原始格式显示。
-t, --topology       显示拓扑结构信息。
-p, --paths          打印完整设备路径
-s, --inverse        反向依赖
-S, --scsi           输出有关 SCSI 设备的信息
-V, --version  输出版本信息并退出

可用列(用于 --output)：
        NAME  设备名

       KNAME  internal kernel device name

     MAJ:MIN  主:次 设备号

      FSTYPE  文件系统类型

  MOUNTPOINT  where the device is mounted

       LABEL  filesystem LABEL

        UUID  filesystem UUID

   PARTLABEL  分区 LABEL

    PARTUUID  分区 UUID

          RA  read-ahead of the device

          RO  只读设备

          RM  removable device

       MODEL  device identifier

      SERIAL  disk serial number

        SIZE  size of the device

       STATE  设备的状态

       OWNER  user name

       GROUP  group name

        MODE  device node permissions

   ALIGNMENT  alignment offset

      MIN-IO  minimum I/O size

      OPT-IO  optimal I/O size

     PHY-SEC  物理扇区大小

     LOG-SEC  逻辑扇区大小

        ROTA  rotational device
            若ROTA=1，则意味该硬盘旋转，则其为机械硬盘；
            若ROTA=0，则意味着该盘为固态硬盘；

       SCHED  I/O scheduler name

     RQ-SIZE  request queue size

        TYPE  device type

    DISC-ALN  discard alignment offset

   DISC-GRAN  discard granularity

    DISC-MAX  discard max bytes

   DISC-ZERO  忽略零数据

       WSAME  write same max bytes

         WWN  unique storage identifier

        RAND  adds randomness

      PKNAME  internal parent kernel device name

        HCTL  Host:Channel:Target:Lun for SCSI

        TRAN  device transport type

         REV  device revision

      VENDOR  device vendor

## 例子

- 
```shell
lsblk

NAME   MAJ:MIN rm   SIZE RO type mountpoint
sda      8:0    0 232.9G  0 disk 
├─sda1   8:1    0  46.6G  0 part /
├─sda2   8:2    0     1K  0 part 
├─sda5   8:5    0   190M  0 part /boot
├─sda6   8:6    0   3.7G  0 part [SWAP]
├─sda7   8:7    0  93.1G  0 part /data
└─sda8   8:8    0  89.2G  0 part /personal
sr0     11:0    1  1024M  0 rom

1. NAME ：这是块设备名。
2. MAJ:MIN ：本栏显示主要和次要设备号。
3. RM ：本栏显示设备是否可移动设备。注意，在本例中设备sdb和sr0的RM值等于1，这说明他们是可移动设备。
4. SIZE ：本栏列出设备的容量大小信息。例如298.1G表明该设备大小为298.1GB，而1K表明该设备大小为1KB。
5. RO ：该项表明设备是否为只读。在本案例中，所有设备的RO值为0，表明他们不是只读的。
6. TYPE ：本栏显示块设备是否是磁盘或磁盘上的一个分区。在本例中，sda和sdb是磁盘，而sr0是只读存储（rom）。
7. MOUNTPOINT ：本栏指出设备挂载的挂载点。
```

- 
```shell
lsblk -lpnb -o NAME,SIZE,ROTA,MOUNTPOINT,FSTYPE
```
