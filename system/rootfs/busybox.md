# busybox

- build
```shell
<!-- 构建根文件系统 -->
git clone git://git.busybox.net/busybox
cd busybox
make menuconfig

Settings  --->
  [*] Build static binary (no shared libs)

<!-- 构建根文件系统 -->
make -j$(nproc) && make install

```

- config
```shell
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
```
