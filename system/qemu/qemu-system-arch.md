# qemu-system-arch

- busybox + initramfs + qemu-system-x86_64 (不推荐)
```shell
<!-- qemu 启动 -->
qemu-system-x86_64 \
  -s \
  -kernel ~/linux/arch/x86_64/boot/bzImage \
  -initrd  ~/busybox/initramfs.cpio.gz \
  --append "nokaslr root=/dev/ram init=/init console=ttyS0" \
  -nographic 


<!-- debug -->
add-auto-load-safe-path ~/linux/scripts/gdb/vmlinux-gdb.py


:~$ gdb vmlinux
(gdb) target remote:1234
Remote debugging using :1234
0xffffffff81bf73ee in native_safe_halt () at ./arch/x86/include/asm/irqflags.h:51
51      asm volatile("sti; hlt": : :"memory");
(gdb)
```



## buildroot + qemu
```shell
<!-- 没有图形界面时，将-serial stdio替换为-nographic -->
<!-- 'nokaslr': 关闭地址空间布局随机化, 的目的是为了能够用gdb调试 -->
<!-- -s: 让 qemu 监听在 tcp::1234 端口上，可以让gdb连接该端口进行调试 -->
<!-- -S: 让 qemu启动后暂停执行，直到通过gdb传入继续执行(continue)命令 -->
<!-- -gdb tcp::1234: 指定 gdb 连接的端口 -->
qemu-system-x86_64 \
    -kernel ./bzImage \
    -hda rootfs.ext4 \
    -append "root=/dev/sda console=ttyS0 nokaslr" \
    -nographic \
    -m 4096 \
    -smp 4 \
    -enable-kvm \
    -net nic \
    -net user,hostfwd=tcp::2222-:22,hostfwd=tcp::8080-:8080,hostfwd=tcp::8000-:8000


<!-- gdb debug -->
qemu-system-x86_64 \
    -kernel ./bzImage \
    -hda rootfs.ext4 \
    -append "root=/dev/sda console=ttyS0 nokaslr" \
    -nographic \
    -m 4096 \
    -smp 4 \
    -enable-kvm \
    -net nic \
    -net user,hostfwd=tcp::2222-:22,hostfwd=tcp::8080-:8080,hostfwd=tcp::8000-:8000 \
    -s -S



gdb /run/media/black/Data/Documents/linux_debug/linux-4.19.315/vmlinux
(gdb) add-auto-load-safe-path /run/media/black/Data/Documents/linux_debug/linux-4.19.315/scripts/gdb/vmlinux-gdb.py
(gdb) target remote 127.0.0.1:1234
Remote debugging using 172.17.0.1:12340x000000000000fff0 in exception_stacks ()
(gdb) c
Continuing.
```


- usb start
```shell
sudo qemu -m 128 -hda /dev/sdd
```

- iso start
```shell
dd if=/dev/zero of=ubuntu.img bs=1G count=30

qemu-system-x86_64 -smp 4 -m 2048 -cdrom /run/media/black/Data/Downloads/iso/archlinux-x86_64.iso ubuntu.img
qemu-system-x86_64 -smp 4 -m 2048  ubuntu.img

qemu-system-x86_64 -m 2048 -smp 2 --enable-kvm ubuntu-20.04.5-amd64.img -cdrom ubuntu-20.04.5-live-server-amd64.iso

<!-- 安装好后，可以用下面的命令启动： -->
qemu-system-x86_64 -m 2048 -smp 2 --enable-kvm ubuntu-20.04.5-amd64.img

qemu-system-x86_64 -smp 4 -m 2048 -cdrom /run/media/black/Data/Downloads/iso/CentOS-7-x86_64-Minimal-2009.iso centos7.img

qemu-system-x86_64 -enable-kvm -smp 4 -m 2048 -nographic centos7.img
```
