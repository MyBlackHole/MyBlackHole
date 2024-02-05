# grub

- 查看可用设备
```shell
grub> ls

(hd0) (hd0,msdos2) (hd0,msdos1)

(hd0)表示第一个硬盘，(hd0,msdos2)和(hd0,msdos1)表示该硬盘上的第二个和第一个分区
```

- 查看目录
```shell
grub> ls (hd0,msdos1)/
```

- 引导启动内核 (如果是其他特殊文件系统需要载入 mod)
```shell
<!-- 设置 root 设备(指定 boot 设备位置) -->
grub> set root=(hd0,msdos1)
<!-- 载入内核 -->
grub> linux /boot/vmlinuz root=/dev/sda1 (cat (hd0,0)/etc/fstab 获取)
<!-- 载入 initrd 镜像 -->
grub> initrd /boot/initrd.img
<!-- 启动系统 -->
grub> boot
```

- 引导 zfs 文件系统 root 系统镜像
```shell
recordfail
load_video
gfxmode ${linux_gfx_mode}
insmod gzio
if [ "${grub_platform}" = xen ]; then insmod xzio; insmod lzopio; fi
insmod part_gpt
insmod zfs
search --no-floppy --fs-uuid --set=root 07849a7005302683
linux	"/BOOT/ubuntu_ty4pf5@/vmlinuz-6.2.0-39-generic" root=ZFS="rpool/ROOT/ubuntu_ty4pf5" ro quiet splash ${vt_handoff}
initrd	"/BOOT/ubuntu_ty4pf5@/initrd.img-6.2.0-39-generic"
```
