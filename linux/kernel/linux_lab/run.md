# run

```shell
make B=vexpress-a9
# make qemu
# make qemu-save

# 启用 source debug info
make debug CONFIG_DEBUG_INFO=y
```

# 文件共享
- rootfs 
```shell
# 本质时重新打包制作 rootfs
mkdir src/system/root/
touch src/system/root/new_file
make root-rebuild
make boot
```

- nfs
```shell
# 开发环境(开发板)
make boot ROOTDEV=nfs

# 主机 NFS 目录
docker exec -it linux-lab-21787-5771af bash
root@linux-lab:/labs/linux-lab# ip route
default via 172.30.19.1 dev br0
172.30.0.0/16 dev br0 proto kernel scope link src 172.30.153.240

ubuntu@linux-lab:/labs/linux-lab$ make env-dump VAR=ROOTDIR
#[ arm/vexpress-a9 ]:
     ROOTDIR="/labs/linux-lab/build/arm/vexpress-a9/bsp/root/2021.08.2/rootfs"
```

- 其他根文件系统
```shell
docker run -it tinylab/arm32v7-ubuntu

(host)$ sudo apt-get install -y qemu-user-static
(host)$ tools/root/docker/extract.sh tinylab/arm32v7-ubuntu arm
(host)$ tools/root/docker/extract.sh tinylab/arm64v8-ubuntu arm

# 用户名和密码均为 root
(lab )$ make boot b=arm/vexpress-a9 U=0 V=1 MEM=1024M ROOTDEV=nfs ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm32v7-ubuntu

# 用户名和密码均为 root
(lab )$ make boot b=aarch64/virt V=1 ROOTDEV=nfs ROOTFS=$PWD/prebuilt/fullroot/tmp/tinylab-arm64v8-ubuntu
```

- 打补丁
```shell
make kernel-patch

或

make patch kernel
```

- 启动
```shell
# 非窗口
make boot

# 窗口
make b=pc boot G=1 LINUX=v5.1 BUILDROOT=2019.11
make b=versatilepb boot G=1 LINUX=v5.1 BUILDROOT=2016.05
make b=g3beige boot G=1 LINUX=v5.1 BUILDROOT=2016.05
make b=malta boot G=1 LINUX=v2.6.36 BUILDROOT=2016.05
make b=vexpress-a9 boot G=1 LINUX=v4.6.7 BUILDROOT=2016.05 // LINUX=v3.18.39 works too
```

- 内核模块
```shell
# 获取模块状态
make kernel-getconfig m=minix_fs

# 使能内核模块
make kernel-setconfig m=minix_fs

y	编译内核中的模块或者使能其他内核选项
c	以插件方式编译内核模块，类似 m 选项
o	以插件方式编译内核模块，类似 m 选项
n	关闭一个内核选项
s	RTC_SYSTOHC_DEVICE="rtc0"，设置 rtc 设备为 rtc0
v	PANIC_TIMEOUT=5, 设置内核 panic 超时为 5 秒


# 使用内核模块 (m=hello, 使用 hello )
make modules
make modules-install
make root-rebuild    // not need for nfs boot
make boot

# 列出 src/modules/ 和 boards/<BOARD>/bsp/modules/ 路径下的所有模块：
make modules-list
```

- 切换开发板
```shell
make BOARD=x86_64/pc
```

- 切换内核
```shell
make config LINUX=v6.1.1

// 清理干净，方便启动一个全新的测试
$ make kernel-cleanall

// 开展测试，这里的小写是为了节省时间，并且该 feature 设定不会保存，仅当次测试有效
$ make test f=rust m=rust_minimal
```
