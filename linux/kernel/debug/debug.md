# debug kernel

```shell
# linux
make menuconfig # 会生成 .config
lvim .config
CONFIG_DEBUG_INFO=y
***.pem # 都去了

make bzImage

<!-- # busybox -->
<!-- make defconfig -->

<!-- make menuconfig -->
<!--   [*] Build static binary (no shared libs)                                                                                                                                                     │ │ -->
<!-- # 或 -->
<!-- sudo apt install busybox -->

<!-- mkdir ramdisk -->
<!-- cd ramdisk -->

# buildroot
make menuconfig
“Target Options” → “Target Architecture” → “Filesystem images” → “ext2/3/4 root file system” → “ext4”

# 运行
qemu-system-x86_64 -kernel arch/x86/boot/bzImage -boot c -m 2049M -hda ../buildroot/output/images/rootfs.ext4 -append "root=/dev/sda rw console=ttyS0,115200 acpi=off nokaslr"-serial stdio -display none

username: root


# debug
qemu-system-x86_64 -s -kernel arch/x86/boot/bzImage -boot c -m 2049M -hda ../buildroot/output/images/rootfs.ext2 -append "root=/dev/sda rw console=ttyS0,115200 acpi=off nokaslr" -serial stdio -display none
gdb ./vmlinux
(gdb)target remote :1234
```
