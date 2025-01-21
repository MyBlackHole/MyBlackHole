# sys

查看系统信息

```shell
crash> sys
      KERNEL: usr/lib/debug/lib/modules/4.19.90-2107.6.0.0220.22.oe1.bclinux.x86_64/vmlinux  [TAINTED]
    DUMPFILE: /home/black/Downloads/vmcore/vmcore  [PARTIAL DUMP]
        CPUS: 128
        DATE: Thu Jan 16 10:32:10 CST 2025
      UPTIME: 41 days, 18:12:36
LOAD AVERAGE: 1.22, 0.72, 0.64
       TASKS: 2206
    NODENAME: nmg-pod22-cdm-store-222-112
     RELEASE: 4.19.90-2107.6.0.0220.22.oe1.bclinux.x86_64
     VERSION: #1 SMP Thu Sep 28 09:31:03 CST 2023
     MACHINE: x86_64  (1999 Mhz)
      MEMORY: 767.9 GB
       PANIC: "Kernel panic - not syncing: Hard LOCKUP"
```

- 查看所有系统调用的实现
```shell
crash> sys -c
NUM  SYSTEM CALL                FILE AND LINE NUMBER
  0  __x64_sys_read             ../fs/read_write.c: 589
  1  __x64_sys_write            ../fs/read_write.c: 610
  2  __x64_sys_open             ../fs/open.c: 1098
  3  __x64_sys_close            ../fs/open.c: 1179
  4  __x64_sys_newstat          ../fs/stat.c: 344
  5  __x64_sys_newfstat         ../fs/stat.c: 382
  6  __x64_sys_newlstat         ../fs/stat.c: 355
  7  __x64_sys_poll             ../fs/select.c: 1049
  8  __x64_sys_lseek            ../fs/read_write.c: 324
  9  __x64_sys_mmap             ../arch/x86/kernel/sys_x86_64.c: 91
 10  __x64_sys_mprotect         ../mm/mprotect.c: 611
 11  __x64_sys_munmap           ../mm/mmap.c: 3187
 12  __x64_sys_brk              ../mm/mmap.c: 189
 13  __x64_sys_rt_sigaction     ../kernel/signal.c: 3820
 14  __x64_sys_rt_sigprocmask   ../kernel/signal.c: 2872
 15  __ia32_sys_rt_sigreturn    ../arch/x86/kernel/signal.c: 643
 16  __x64_sys_ioctl            ../fs/ioctl.c: 710
 17  __x64_sys_pread64          ../fs/read_write.c: 636
 18  __x64_sys_pwrite64         ../fs/read_write.c: 662
 19  __x64_sys_readv            ../fs/read_write.c: 1103
 20  __x64_sys_writev           ../fs/read_write.c: 1109
 21  __x64_sys_access           ../fs/open.c: 447
 22  __x64_sys_pipe             ../fs/pipe.c: 878
 23  __x64_sys_select           ../fs/select.c: 725
```
