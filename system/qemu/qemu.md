# qemu

- 退出
```shell
ctrl+a x # exit qemu
```

- 安装
```shell
yay -S qemu-full
```

- 安装 linux 系统环境
```shell
paru -S bc qemu-full ncurses-devel

qemu-system-x86_64 -hda /path/to/your/linux/image.img

cd linux

make menuconfig # 配置内核

<!-- debug 相关 -->
Kernel hacking  --->
  Compile-time checks and compiler options  --->
    [*] Compile the kernel with debug info
    [*]   Provide GDB scripts for kernel debugging

<!-- 关闭内核地址空间布局随机化 -->
Processor type and features  --->
  [ ]   Randomize the address of the kernel image (KASLR)

<!-- 关闭签名验证 -->
-*- Cryptographic API  --->
  Certificates for signature checking  --->
    (debian/canonical-certs.pem) Additional X.509 keys for default system keyring

<!-- 构建内核 -->
make -j$(nproc)


<!-- 构建根文件系统 -->
git clone git://git.busybox.net/busybox

cd busybox

make menuconfig

Settings  --->
  [*] Build static binary (no shared libs)

<!-- 构建根文件系统 -->
make -j$(nproc) && make install

<!-- 制作 initramfs -->
cd ./_install
mkdir dev 
sudo mknod dev/console c 5 1
sudo mknod dev/ram b 1 0 
touch init
chmod +x init

<!-- init 脚本内容 -->
#!/bin/sh
echo "INIT SCRIPT"
mkdir /proc
mkdir /sys
mount -t proc none /proc
mount -t sysfs none /sys
mkdir /tmp
mount -t tmpfs none /tmp
mknod -m 666 /dev/ttyS0 c 4 64
echo -e "\nThis boot took $(cut -d' ' -f1 /proc/uptime) seconds\n"
setsid cttyhack /bin/sh
exec /bin/sh

<!-- cpio 打包 -->
find . -print0 | cpio --null -ov --format=newc | gzip -9 >  ../initramfs.cpio.gz


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



## buildroot | qemu
```shell
<!-- 没有图形界面时，将-serial stdio替换为-nographic -->
<!-- ‘nokaslr’ 的目的是为了能够用gdb调试 -->
<!-- sudo qemu-system-x86_64 -kernel ./bzImage -hda rootfs.ext4 -append "root=/dev/sda console=ttyS0 nokaslr" -serial stdio -->
sudo qemu-system-x86_64 -kernel ./bzImage -hda rootfs.ext4 -append "root=/dev/sda console=ttyS0 nokaslr" -nographic

<!-- gdb debug -->
sudo qemu-system-x86_64 -kernel ./bzImage -hda rootfs.ext4 -append "root=/dev/sda console=ttyS0 nokaslr" -serial stdio -s -S
<!-- -s: 让 qemu 监听在 tcp::1234 端口上，可以让gdb连接该端口进行调试 -->
<!-- -S: 让 qemu启动后暂停执行，直到通过gdb传入继续执行(continue)命令 -->

gdb /root/linux-4.19.108/vmlinux(gdb) add-auto-load-safe-path /root/linux-4.19.108/scripts/gdb/vmlinux-gdb.py(gdb) target remote 172.17.0.1:1234Remote debugging using 172.17.0.1:12340x000000000000fff0 in exception_stacks ()(gdb) cContinuing.


qemu-system-x86_64 -kernel ./bzImage -hda rootfs.ext4 -append "root=/dev/sda console=ttyS0 nokaslr" -nographic -m 4096 -smp 4 -enable-kvm -net nic -net user,hostfwd=tcp::2222-:22,hostfwd=tcp::8080-:8080,hostfwd=tcp::8000-:8000
```
