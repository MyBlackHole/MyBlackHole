# debug

[不错的文章](https://www.jianshu.com/p/8e1ec1f911dd)
```shell
yum install kernel-debug
```

- 编译Linux内核源代码
```shell
<!-- 安装源码包 -->
rpm –ivh kernel-3.10.0-1160.el7.src.rpm

<!-- useradd -s /sbin/nologin mockbuild -->
<!-- yum install rpm-build -->
<!-- 通过rpmbuild -bp完成源码的patch和config工作（-bp表示prepare） -->
rpmbuild -bp ~/rpmbuild/SPECS/kernel.spec


<!-- 进入Linux内核源码目录 -->
cd ~/rpmbuild/BUILD/kernel-3.10.0-1160.el7/linux-3.10.0-1160.el7.centos.x86_64/
make bzImage && make modules


<!-- 生成gdb需要的vmlinux和待安装的新内核bzImage（arch/x86_64/boot/bzImage） -->

```

- 安装新内核

编译完成需要将生成的内核（bzImage）和modules安装到虚拟机中去，一种方式是制作rpm包然后拷贝到虚拟机中去安装:
make binrpm-pkg INSTALL_MOD_STRIP=1

```shell
<!-- 1. 进入源码目录，修改代码后编译 -->
cd ~/rpmbuild/BUILD/kernel-3.10.0-1160.el7/linux-3.10.0-1160.el7.centos.x86_64
make bzImage
make modules (第一次编译安装新内核时需要)


<!-- 2. QEMU启动虚拟机 -->
<!-- *注意此时不需要’-s -S’选项，同时可以新增’-enable-kvm’和’-smp 2’来提高虚拟机的性能 -->
qemu-system-x86_64 -m 4096 -vnc 0.0.0.0:1 centos.img.qcow2 -net nic -net tap,script=/etc/qemu-ifup1 -enable-kvm -smp 2

<!-- 3. 在虚拟机中mount Host导出的源码目录  -->
showmount -e 192.168.10.115
Export list for 192.168.10.115:
/root/rpmbuild/BUILD 192.168.10.0/24
mount -t nfs 192.168.10.115:/root/rpmbuild/BUILD /mnt/nfs/

<!-- 4. 进入源码目录安装新编译的内核 -->

<!-- (第一次编译安装新内核时需要) -->
make modules_install INSTALL_MOD_STRIP=1
make install

<!-- 进入系统配置 nokaslr -->
vi /etc/default/grub
GRUB_CMDLINE_LINUX 追加 nokaslr

grub2-mkconfig -o /boot/grub2/grub.cfg
```

- 开始调试内核

```shell
<!-- 1. 检查网络配置 -->
<!-- 2. 在会话1中通过QEMU创建虚拟机 -->

cd /home/gj/virt/qemu-kernel
qemu-system-x86_64 -m 4096 -vnc 0.0.0.0:1 centos.img.qcow2 -net nic -net tap,script=/etc/qemu-ifup1 -s -S

<!-- 3. 会话2 中进入源码目录并 gdb -->
cd ~/rpmbuild/BUILD/kernel-3.10.0-1160.el7/linux-3.10.0-1160.el7.centos.x86_64
gdb vmlinux

<!-- 4. 在gdb中连接QEMU的gdbserver并设置断点 -->
(gdb) target remote localhost:1234
Remote debugging using localhost:1234
0x0000000000000000 in irq_stack_union ()
(gdb) break yfs_mount
Breakpoint 1 at 0xffffffff812b259f: file fs/yfs/super.c, line 777.
(gdb) c
Continuing.
(gdb) info source
Current source file is fs/read_write.c
Compilation directory is /root/rpmbuild/BUILD/kernel-3.10.0-1160.el7/linux-3.10.0-1160.el7.x86_64
Source language is c.
Producer is GNU C 4.8.5 20150623 (Red Hat 4.8.5-44) -m64 -mpreferred-stack-boundary=3 -mtune=generic -mno-red-zone -mcmodel=kernel -maccumulate-outgoing-args -mno-sse -mno-mmx -mno-sse2 -mno-3dnow -mno-avx -mindirect-branch=thunk-extern -mindirect-branch-register -mfentry -march=x86-64 -g -O2 -std=gnu90 -p -fno-strict-aliasing -fno-common -fno-delete-null-pointer-checks -funit-at-a-time -fno-asynchronous-unwind-tables -fstack-protector-strong -fno-omit-frame-pointer -fno-optimize-sibling-calls -fno-inline-functions-called-once -fno-strict-overflow -fconserve-stack.
Compiled with DWARF 4 debugging format.
Does not include preprocessor macro info.
(gdb) set substitute-path /root/rpmbuild/BUILD/kernel-3.10.0-1160.el7/linux-3.10.0-1160.el7.x86_64 /run/media/black/Data/Documents/linux_debug/linux3/linux-3.10.0-1160.el7
```

