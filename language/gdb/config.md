# config


- 普通用户启用 attach all processes 权限
```shell
sysctl kernel/yama/ptrace_scope
kernel.yama.ptrace_scope = 1
<!--修改为 0-->
sudo sysctl -w kernel.yama.ptrace_scope=0

<!--会话级修改-->
echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope

<!--永久修改-->
sudo vim /etc/sysctl.conf
kernel.yama.ptrace_scope = 0

<!--selinux 拦击-->
dmesg | grep "avc"
journalctl -k | grep "DENIED"
```

- 普通用户 attach, 调试 pid 进程
```shell
sudo sysctl kernel.yama.ptrace_scope=0
gdb attach [pid] 

-----

sudo gdb attach 766047
```

- 修改编译路径
```shell
<!-- 修改 root1 目录为 root 目录 -->
set substitute-path /root1/ /root/

show substitute-path

set substitute-path / /root/fs-backup/fsdeamon/

unset substitute-path /root1/
```

- 修改 debug 路径
```shell
show debug-file-directory
set debug-file-directory /root/fs-backup/fsdeamon/


unset debug-file-directory /root/fs-backup/fsdeamon/
```

- 设置源码路径
```shell
<!-- 最好直接进入源码目录，然后执行 gdb 命令(前提是编译时基于源码目录编译) -->

directory fsdeamon/
directory /path/to/source:/another/path/to/source

<!-- gdb -q a.out -d /search/code/some -->
```

- 配置 lib 路径
```shell
(gdb) set solib-search-path /my/special/libs
(gdb) set sysroot /my/special/sysroot
(gdb) set environment LD_LIBRARY_PATH /my/special/libs
```

- 修改显示长度
```shell
show print elements
set print elements 0
```

- 附加到进程带上执行文件路径
```shell
gdb /xxxx/xxxx/xxxx/test -p 1234
```

- 多线程调试
```shell
info threads   查看当前进程的线程。
    GDB会为每个线程分配一个ID, 后面操作线程的时候会用到这个ID.
    前面有*的是当前调试的线程.
thread <ID>  切换调试的线程为指定ID的线程。

break ChangeTrackup thread all  为所有经过 ChangeTrackup 的线程设置断点。
set scheduler-locking off|on|step    
      在使用step或者continue命令调试当前被调试线程的时候，其他线程也是同时执行的,
      怎么只让被调试程序执行呢？
      通过这个命令就可以实现这个需求。
         off      不锁定任何线程，也就是所有线程都执行，这是默认值。
         on       只有当前被调试程序会执行。
         step     在单步的时候，除了next过一个函数的情况
                  (熟悉情况的人可能知道，这其实是一个设置断点然后continue的行为)以外，
                  只有当前线程会执行。
thread apply ID1 ID2 command        让一个或者多个线程执行GDB命令command
thread apply all command            让所有被调试线程执行GDB命令command。
```

- 设置多进程调试
```shell
<!-- 调试子进程 -->
set follow-fork-mode child

<!-- 禁用 fork 进程 -->
set detach-on-fork off
```

- 传递参数
```shell
gdb --args ./test/unit/core configurationDecode/badAddress
# 或
(gdb)r arg1 arg2
# 或
(gdb)set args arg1 arg2
```

- 使用 .gdbinit
```
# gdb
set disassemble-next-line on
b _start
target remote : 1234
c

# shell
gdb -x .gdbinit ./test/unit/core
```

- 设置源码目录
```
(gdb) directory /media/black/Data/Documents/github/C/tbox
Source directories searched: /media/black/Data/Documents/github/C/tbox:/media/black/Data/Documents/C/C_learn/out/obj/tbox_learn/heap_test1:$cdir:$cwd
```

- 调试 python 进程
```shell
gdb -p pid
source Tools/gdb/libpython.py # cpython 里有
```
