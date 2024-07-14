# build


## build
```shell
# 需要关闭 python 虚拟环境
./autogen.sh
./configure CFLAGS='-g -O0' --enable-debug --enable-debuginfo
./configure CFLAGS='-g -O0' (./configure --enable-debug)
# ./configure CFLAGS='-g -O0' --prefix=/media/black/Data/lib/zfs/master
make all
```

## 安装
```shell
<!-- centos -->
<!-- 现在安装内核开发和zfs包 -->
<!-- 内核开发的软件包是需要ZFS建立模块和插入到内核中 -->
yum install epel-release
yum install kernel-devel zfs

# ubuntu
sudo apt install zfsutils-linux
```

## 配置到 linux kernel mod 配置编译
```shell
<!-- zfs-0.8.6: 有问题，应该是 gcc 版本过高 -->
<!-- zfs-2.0.1 -->

<!-- cd linux-3.19: 有问题 -->
<!-- make scripts -->

<!-- ./autogen.sh -->
<!-- ./configure \ -->
<!--     --enable-linux-builtin=yes \ -->
<!--     --with-linux=/run/media/black/Data/Documents/linux_debug/linux-3.19 \ -->
<!--     --with-linux-obj=/run/media/black/Data/Documents/linux_debug/linux-3.19 -->
<!-- ./copy-builtin /run/media/black/Data/Documents/linux_debug/linux-3.19 -->

./configure \
    --enable-linux-builtin=yes \
    --with-linux=/run/media/black/Data/Documents/linux_debug/linux-4.19.315 \
    --with-linux-obj=/run/media/black/Data/Documents/linux_debug/linux-4.19.315

./copy-builtin /run/media/black/Data/Documents/linux_debug/linux-4.19.315

<!-- linux: 直接编译到内核镜像, 会缺少用户层工具 -->
cd linux-4.19.315
make menuconfig
File systems
    <*> ZFS filesystem support (NEW)
make

<!-- zfs -->
cd zfs-2.1.7/
make -s -j8
<!-- make install -->
make dist-gzip

❯ ls -alh zfs-2.2.4.tar.gz
Permissions Size User  Date Modified Name
.rw-r--r--   34M black  6 Jul 09:09   zfs-2.2.4.tar.gz

cd kernel/
make clean
make modules -j8
make modules_install
make -j 8 bindeb-pkg LOCALVERSION=-custom

dpkg -i linux*
u-boot-update


# ls -alh /sys/module/zfs/
total 0
drwxr-xr-x    3 root     root           0 Jul  6 00:48 .
drwxr-xr-x  115 root     root           0 Jul  6 00:48 ..
drwxr-xr-x    2 root     root           0 Jul  6 00:48 parameters
--w-------    1 root     root        4.0K Jul  6 00:48 uevent
-r--r--r--    1 root     root        4.0K Jul  6 00:48 version
```
