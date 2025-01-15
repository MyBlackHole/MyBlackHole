# run

## modules

- debug modules
```shell
sudo apt install qemu-utils
sudo apt-get install xfsprogs

<!-- 配置默认开启 debug\修改根路径 -->
<!-- make CONFIG_DEBUG_INFO=y (不起作用) -->
make ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu

<!-- 配置需要启用模块 -->
make menuconfig

<!-- 编译 module 到指定根文件系统 -->
make modules CONFIG_DEBUG_INFO=y ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu
make modules-install CONFIG_DEBUG_INFO=y ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu

<!-- make debug CONFIG_DEBUG_INFO=y -->
make debug CONFIG_DEBUG_INFO=y b=arm/vexpress-a9 U=0 V=1 MEM=1024M ROOTDEV=nfs ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu

<!-- 给 qemu 传递额外参数 (不生效) -->
QOPTS="-serial vc -serial vc"
QOPTS="-hda ./disk.img"


<!-- 创建一个文件镜像 200M -->
dd if=/dev/zero of=disk1.img bs=1M count=200
---
qemu-img create -f raw ./disk1.img 4G

<!-- 启动内核 -->
make debug QOPTS="-hda /labs/linux-lab/disk1.img" CONFIG_DEBUG_INFO=y b=arm/vexpress-a9 U=0 V=1 MEM=1024M ROOTDEV=nfs ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu
make debug QOPTS="-drive file=/labs/linux-lab/disk1.img,format=raw,index=0,media=disk" CONFIG_DEBUG_INFO=y b=arm/vexpress-a9 U=0 V=1 MEM=1024M ROOTDEV=nfs ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu

<!-- 启用 loop 块设备 -->
modprobe loop (需要内核模块支持)

<!-- 初始化绑定  -->
mkfs.xfs disk1.img
losetup /dev/loop1 ./disk1.img

<!-- 挂载 -->
mkdir test1
mount -o loop -t xfs /dev/loop1 test1/


flock -n -x /labs/linux-lab/.gdb/.lock sudo prlimit -n1024 qemu-system-arm -M vexpress-a9 -cpu cortex-a9 -m 128M -net nic,model=lan9118 -net tap,script=/etc/qemu-ifup,downscript=/etc/qemu-ifdown -smp 1 -kernel /labs/linux-lab/boards/arm/vexpress-a9/bsp/uboot/v2020.04/u-boot -no-reboot  -drive if=pflash,file=tftpboot/pflash.img,format=raw -nographic   -drive file=/labs/linux-lab/disk1.img,format=raw,index=0,media=disk -s -S || gdb-multiarch /labs/linux-lab/build/arm/vexpress-a9/linux/v6.2/vmlinux
```


- nfsd
```shell
<!-- 配置 nfsd -->
make menuconfig

<!-- 编译 -->
make modules 

make modules-install 
<!-- ROOTFS:指定根文件系统 -->
make modules-install ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu

make root-rebuild    // not need for nfs boot
make boot

make boot b=arm/vexpress-a9 U=0 V=1 MEM=1024M ROOTDEV=nfs ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu
make debug b=arm/vexpress-a9 U=0 V=1 MEM=1024M ROOTDEV=nfs ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu

不支持内核 nfs 根文件系统的目录共享 nfs, 可以用内存文件系统共享
```
