# linux kernel debug

##  debug kernel
```shell
# linux
make menuconfig # 会生成 .config

# 关闭kernel地址随机化
进入Processor type and features-->Randomize the address of the kernel image(KASLR)，去掉勾选。
打开.config文件，搜索X86_NEED_RELOCS字符串，发现不存在，证明关闭kernel地址随机化成功。

lvim .config
CONFIG_DEBUG_INFO=y
CONFIG_GDB_SCRIPTS=y
CONFIG_DEBUG_KERNEL=y
# CONFIG_FRAME_POINTER=y

 Kernel hacking  --->                                                                                     │ │
    [*] Kernel debugging                              
    [ ]   Miscellaneous debug code                    
        Compile-time checks and compiler options  --->

***.pem # 都去了

make bzImage
```

##  文件系统镜像
### busybox
```shell
make defconfig

make menuconfig
Settings  --->
  [*] Build static binary (no shared libs)                                                                                                                                                     │ │
# 或
sudo apt install busybox

dd if=/dev/zero of=rootfs.img bs=1M count=10
# 初始化
mkfs.ext4 rootfs.img
mkdir fs
# 挂载
sudo mount -t ext4 -o loop rootfs.img ./fs
# 构建
sudo make install CONFIG_PREFIX=./fs
cd fs
sudo mkdir proc dev etc home mnt
sudo cp -r ../examples/bootfloppy/etc/* etc/
# 卸载 镜像
cd ..
sudo umount fs
```

- 抽取 ubuntu server 镜像
```shell
debootstrap
```

#### debug run 
- 启动qemu
```shell
sudo modprobe tun

cd /media/black/Data/Documents/linux_debug

qemu-system-x86_64 -kernel arch/x86_64/boot/bzImage \
    -hda rootfs.img  \
    -hdb disk.img  \
    -append "root=/dev/sda console=ttyS0" \
    -net nic,model=e1000 -net tap,script=/etc/qemu-ifup,downscript=/etc/qemu-ifdown \
    -nographic

-kernel # 指定编译好的内核镜像
-hda # 指定硬盘映射到 /dev/sda
-hdb # 指定硬盘映射到 /dev/sdb (如果 -hdc 的话还是 sdb 按顺序的)
-append "root=/dev/sda" 指示根文件系统 console=ttyS0 把QEMU的输入输出定向到当前终端上
-nographic 不使用图形输出窗口
-s -S 是 -gdb tcp::1234缩写，监听1234端口，在GDB中可以通过target remote localhost:1234连接
```
- 内核函数调试
```shell
qemu-system-x86_64 -kernel ../github/C/linux/arch/x86_64/boot/bzImage \
    -drive format=raw,file=rootfs.img,media=disk \
    -append "root=/dev/sda console=ttyS0" \
    -s -S  -smp 1 \
    -net nic,model=lan9118 -net tap,script=/etc/qemu-ifup,downscript=/etc/qemu-ifdown \
    -nographic

# C=A X

gdb ../github/C/linux/vmlinux
target remote :1234
```

- 添加共享磁盘
```shell
dd if=/dev/zero of=ext4.img bs=512 count=131072
mkfs.ext4 ext4.img
# -hdb 增加一个磁盘。
qemu-system-x86_64 -kernel ~/linux-4.14.191/arch/x86_64/boot/bzImage  \
    -hda ~/busybox-1.32.0/rootfs.img  \
    -append "root=/dev/sda console=ttyS0" \
    -s  -smp 1 -nographic \
    -hdb ~/shadisk/ext4.img

# debug linux 里
mount /dev/sdb /mnt

# 宿主机器
sudo mount -t ext4 -o lopp ext4.img ./share
```

### buildroot
```shell
make menuconfig
“Target Options” → “Target Architecture” → “Filesystem images” → “ext2/3/4 root file system” → “ext4”
```

#### debug run 
```
# 运行
qemu-system-x86_64 -kernel arch/x86/boot/bzImage -boot c -m 2049M -hda ../buildroot/output/images/rootfs.ext4 -append "root=/dev/sda rw console=ttyS0,115200 acpi=off nokaslr"-serial stdio -display none

username: root

# debug
qemu-system-x86_64 -s -S -kernel arch/x86/boot/bzImage -boot c -m 2049M -hda ../buildroot/output/images/rootfs.ext2 -append "root=/dev/sda rw console=ttyS0,115200 acpi=off nokaslr" -serial stdio -display none
gdb ./vmlinux
(gdb)target remote :1234
```

- 启用 linux kernel gdb 脚本
```shell
(gdb) add-auto-load-safe-path /home/lgj/work/linux 
(gdb) source /home/lgj/work/linux/vmlinux-gdb.py
```
