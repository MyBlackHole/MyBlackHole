# crash

使用crash来调试vmcore，至少需要两个参数：

未压缩的内核映像文件vmlinux。ubuntu默认位于/usr/lib/debug/lib/modules/$(uname -r)/vmlinux，由内核调试信息包提供。
内存转储文件vmcore，由kdump或sysdump转存的内核奔溃现场快照。

对于ARM平台来说，除上述参数之外，一般还需要指定如下参数：

phys_offset: RAM的起始物理地址
vabits_actual: 实际的虚拟地址空间大小(总线位宽)
kimage_voffset: kernel image映射的物理地址偏移

- docs
```shell
<!--内容不错-->
git@github.com:yifengyou/crash.git
```

- build
```shell
make target=ARM64
```


```shell
./crash_arm64 -m vabits_actual=39 -m phys_offset=0x80000000 -m kimage_voffset=0xffffffbf90000000 ./vmlinux ./vmcore

# KERNEL表示调试用内核的位置和版本信息
# DUMPFILE: 表示所分析的内存转储镜像
# CPUS: 表示本机的 CPU 数目
# DATE: 表示内核崩溃发生的时间
# UPTIME: 表示内核已正常运行的时间
# LOAD AVERAGE: 表示内核崩溃时系统的负载
# TASKS: 表示内核崩溃时系统运行的任务数
# NODENAME: 表示内核崩溃的机器的主机名
# RELEASE: 表示内核的发布版本
# VERSION: 表示内核的其他版本信息
# MACHINE: 表示 CPU 的架构和主频信息
# MEMORY: 表示发生内核崩溃的系统的内存大小
# PANIC: 表示内核崩溃的类型
# PID: 表示导致内核崩溃的进程号；
# COMMAND: 表示导致内核崩溃的进程名称
# TASK: 表示导致内核崩溃的进程访问的内存地址；
# CPU: 表示导致内核崩溃的进程占用的 CPU 数目；
# STATE: 表示导致内核崩溃的进程的运行状态。

# # PANIC，它告诉我们内核奔溃的原因
# # # SysRq。即通过系统请求造成的内核崩溃，如上面测试用的命令。
# # # Oops。表示内核发生了不可预期的或不正确的行为，这时会杀死相应的进程，内核可能恢复正常，也可能处于一种不确定的状态，并进而导致内核的 Panic。
# # # Panic。 内核崩溃，即发生了严重且不可修复的错误， 如发生了非法的地址访问， 强制加载或卸载内核模块，以及硬件错误等等。
```


```shell
paru -S crash
<!--yum install kernel-debuginfo-common-->
<!--yum install kernel-debuginfo-->
[root@worker-67 fsbackup_kernel_4.x]# ls -al /usr/lib/debug/usr/lib/modules/4.19.90-2107.6.0.0251.43.oe1.bclinux.aarch64/
总用量 219372
drwxr-xr-x  4 root root      4096 12月  6 14:47 .
drwxr-xr-x  3 root root      4096 12月  6 14:46 ..
drwxr-xr-x 12 root root      4096 12月  6 14:46 kernel
drwxr-xr-x  2 root root      4096 12月  6 14:46 vdso
-rwxr-xr-x  1 root root 224620056  7月  2 14:29 vmlinux

paru -S kdumpst(kdump 服务需要)


git clone git@github.com:crash-utility/crash.git


cat /proc/cmdline
BOOT_IMAGE=/vmlinuz-linux root=UUID=dc10bd9d-d7ca-4ba5-816b-683230108cb6 rw loglevel=3 quiet

<!--需要配置 crashkernel=auto 到启动参数中，-->
<!--这样 crash 工具才会自动检测并使用可用内存作为 crash 缓冲区。-->

<!--触发 panic-->
echo c > /proc/sysrq-trigger

系统重新启动
crash vmcore /usr/lib/debug/usr/lib/modules/xxxxxxxxxxxxxxxxxxxxx/vmlinux (**kernel-debuginfo**)
```

## 操作命令

| 命令      | 功能                              |
| ------- | ------------------------------- |
| *       | 指针快捷健                           |
| alias   | 命令快捷键                           |
| ascii   | ASCII码转换和码表                     |
| bpf     | eBPF - extended Berkeley Filter |
| bt      | 堆栈查看                            |
| btop    | 地址页表转换                          |
| dev     | 设备数据查询                          |
| dis     | 返汇编                             |
| eval    | 计算器                             |
| exit    | 退出                              |
| extend  | 命令扩展                            |
| files   | 打开的文件查看                         |
| foreach | 循环查看                            |
| fuser   | 文件使用者查看                         |
| gdb     | 调用gdb执行命令                       |
| help    | 帮助                              |
| ipcs    | 查看system V IPC工具                |
| irq     | 查看irq数据                         |
| kmem    | 查看Kernel内存                      |
| list    | 查看链表                            |
| log     | 查看系统消息缓存                        |
| mach    | 查看平台信息                          |
| mod     | 加载符号表                           |
| mount   | Mount文件系统数据                     |
| net     | 网络命令                            |
| p       | 查看数据结构                          |
| ps      | 查看进程状态信息                        |
| pte     | 查看页表                            |
| ptob    | 页表地址转换                          |
| ptov    | 物理地址虚拟地址转换                      |
| rd      | 查看内存                            |
| repeat  | 重复执行                            |
| runq    | 查看run queue上的线程                 |
| search  | 搜索内存                            |
| set     | 设置线程环境和Crash内部变量                |
| sig     | 查询线程消息                          |
| struct  | 查询结构体                           |
| swap    | 查看swap信息                        |
| sym     | 符号和虚拟地址转换                       |
| sys     | 查看系统信息                          |
| task    | 查看task_struct和thread_thread信息   |
| timer   | 查看timer队列                       |
| tree    | 查看radix树和rb树                    |
| union   | 查看union结构体                      |
| vm      | 查看虚拟内存                          |
| vtop    | 虚拟地址物理地址转换                      |
| waitq   | 查看wait queue上的进程                |
| whatis  | 符号表查询                           |
| wr      | 改写内存                            |
| q       | 退出                              |


```shell
root@root-PC:/var/crash/202201120843# crash /lib/modules/5.3.18+/build/vmlinuz-5.3.18+ dump.202201120843

      KERNEL: /data/linux-source-5.3.0/vmlinux  //debug 内核
    DUMPFILE: dump.202204091433  [PARTIAL DUMP] //dump文件
        CPUS: 8  //CPU数量
        DATE: Sat Apr  9 14:33:07 2022  //发生dump的日期
      UPTIME: 00:04:03  //表示内核已正常运行的时间
LOAD AVERAGE: 2.31, 0.82, 0.29  //内核崩溃时的系统负载
       TASKS: 343 //内核崩溃时系统运行的任务数
    NODENAME: root-PC //主机名
     RELEASE: 5.3.18+ //内核版本
     VERSION: #2 SMP Sat Apr 9 14:26:36 CST 2022
     MACHINE: x86_64  (2494 Mhz)  //CPU架构与主频信息
      MEMORY: 16 GB  //发生内核崩溃的系统的内存大小
       PANIC: "Kernel panic - not syncing: sysrq triggered crash"  //内核崩溃的类型
         PID: 1341 //导致内核崩溃的进程号
     COMMAND: "bash"  //内核崩溃的进程名称，或命令
        TASK: ffff8ede003bdc00  [THREAD_INFO: ffff8ede003bdc00]  //内核崩溃的进程访问的内存地址
         CPU: 6  //表示导致内核崩溃的进程占用的 CPU 
       STATE: TASK_RUNNING (PANIC)  //表示导致内核崩溃的进程的运行状态
       
```


### vm

当前进程使用的虚拟内存，VMA 代表 vm_area_struct


> https://blog.csdn.net/heshuangzong/article/details/126906923


```shell
log + bt + dis
使用log查看日志，bt查看堆栈情况，dis查看异常时的代码行及反汇编代码。

dev+irq+mod+mount+net+ps+runq+ipcs
查看系统的设备列表，中断分配，模块使用情况，网络配置情况，进程状态，cpu上的运行队列信 息,ipcs共享内存使用情况
```
