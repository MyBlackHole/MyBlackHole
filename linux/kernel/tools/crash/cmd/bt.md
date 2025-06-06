# bt


查看异常时的堆栈

-t: 显示符号信息
-f: 显示栈的所有数据
-l: 显示文件名和行号
pid: 可以显示指定pid进程的backtrace

- 例如
![](imgs/crash.png)

```shell
堆栈执行的函数顺序是由大到小，
大的是最开始执行的系统调用，
小的 #0 是切换到 crashkernel 的执行。
重点关注位置打印出很多寄存器的地址，
标准的信息是 exception RIP 表示出问题时候执行的指令。
我们可以找到 rip, dis -rl RIP(地址)  查看指令源码

crash> dis -rl ffffffff90a61bf6
/usr/src/debug/kernel-3.10.0-957.el7/linux-3.10.0-957.el7.x86_64/drivers/tty/sysrq.c: 134
0xffffffff90a61be0 <sysrq_handle_crash>:        nopl   0x0(%rax,%rax,1) [FTRACE NOP]
0xffffffff90a61be5 <sysrq_handle_crash+5>:      push   %rbp
0xffffffff90a61be6 <sysrq_handle_crash+6>:      mov    %rsp,%rbp
/usr/src/debug/kernel-3.10.0-957.el7/linux-3.10.0-957.el7.x86_64/drivers/tty/sysrq.c: 143
0xffffffff90a61be9 <sysrq_handle_crash+9>:      movl   $0x1,0x7e5721(%rip)        # 0xffffffff91247314
/usr/src/debug/kernel-3.10.0-957.el7/linux-3.10.0-957.el7.x86_64/drivers/tty/sysrq.c: 144
0xffffffff90a61bf3 <sysrq_handle_crash+19>:     sfence
/usr/src/debug/kernel-3.10.0-957.el7/linux-3.10.0-957.el7.x86_64/drivers/tty/sysrq.c: 145
0xffffffff90a61bf6 <sysrq_handle_crash+22>:     movb   $0x1,0x0
```

- 显示调用代码的行号
```shell
bt -l
```

```shell
dis -f
......
 #7 [ffffbd3602893b78] __kmalloc_node at ffffffff9c49a9d3
    ffffbd3602893b80: 0000000000000010 0000000000010246
    ffffbd3602893b90: ffffbd3602893ba8 0000000000000018
    ffffbd3602893ba0: ffffffff9c49a913 ffffffff9c43eba3
    ffffbd3602893bb0: 00000000007000c0 0000000000001150
    ffffbd3602893bc0: 00000000ffffffff 0000000000001150
    ffffbd3602893bd0: ffff9c5d2fb6e618 ffff9c5a0728f100
    ffffbd3602893be0: ffffffff9c43eba3
 #8 [ffffbd3602893be0] kvmalloc_node at ffffffff9c43eba3
    ffffbd3602893be8: ffff9c5a0728f100 ffffffff9c566720
    ffffbd3602893bf8: 0000000000000000 ffffffff9c4f47bc
 #9 [ffffbd3602893c00] single_open_size at ffffffff9c4f47bc
    ffffbd3602893c08: ffff9c5d2eef3d40 ffff9c5d2eef3d40
    ffffbd3602893c18: ffff9c5a0d3e8310 ffff9c5a0728f100
    ffffbd3602893c28: 00000000fffffff4 ffff9c5d2fb6e618
    ffffbd3602893c38: ffffffff9c55ceee
#10 [ffffbd3602893c38] proc_reg_open at ffffffff9c55ceee
    ffffbd3602893c40: ffff9c5a0728f100 ffff9c5a0d3e8310
    ffffbd3602893c50: ffff9c5a0728f110 ffffffff9c55ce80
    ffffbd3602893c60: 0000000000000000 ffff9c5a0728f100
    ffffbd3602893c70: ffffffff9c4c803d
#11 [ffffbd3602893c70] do_dentry_open at ffffffff9c4c803d
    ffffbd3602893c78: ffffbd3602893eb4 0000000000008000
    ffffbd3602893c88: 0000000000000000 0000000000000004
    ffffbd3602893c98: ffffbd3602893d80 ffffffff9c4dc4f6
#12 [ffffbd3602893ca0] path_openat at ffffffff9c4dc4f6
    ffffbd3602893ca8: ffff9c5a22aa416d 0000000000000000
    ffffbd3602893cb8: ffff9c5a00000002 000000419d25cb00
    ffffbd3602893cc8: ffffbd3602893d30 ffffffff9d25cb48
    ffffbd3602893cd8: ffffffff9cabdabe ffff9c5a22aa3778
    ffffbd3602893ce8: 0000000000000001 ffff0a0000000a00
    ffffbd3602893cf8: 06bcacfd3a58bf00 ffff9c5a0cfa2000
    ffffbd3602893d08: 0000000202893da0 ffff9c5a0d3e8310
    ffffbd3602893d18: ffff9c5a01dea2a0 ffff9c5a0654cf30
    ffffbd3602893d28: ffff9c5a0cfa2000 ffffffff9c4f4510
    ffffbd3602893d38: ffff9c5a0cfa2000 06bcacfd3a58bf00
    ffffbd3602893d48: 00000000ffffff9c ffffbd3602893e90
    ffffbd3602893d58: ffffbd3602893d80 ffffbd3602893eb4
    ffffbd3602893d68: 0000000000000001 0000000000000000
    ffffbd3602893d78: ffffffff9c4deadb
......
```


- bt pid 
```shell
crash> bt 965
PID: 965    TASK: ffff9c5a03c2bc00  CPU: 0   COMMAND: "irqbalance"
 #0 [ffffbd36028938a0] machine_kexec at ffffffff9c254a9f
 #1 [ffffbd36028938f8] __crash_kexec at ffffffff9c357af1
 #2 [ffffbd36028939b8] crash_kexec at ffffffff9c3589dd
 #3 [ffffbd36028939d0] oops_end at ffffffff9c22331f
 #4 [ffffbd36028939f0] do_trap at ffffffff9c21fac2
 #5 [ffffbd3602893a30] do_error_trap at ffffffff9c21ffef
 #6 [ffffbd3602893af0] stack_segment at ffffffff9cc0112e
 #7 [ffffbd3602893b78] __kmalloc_node at ffffffff9c49a9d3
 #8 [ffffbd3602893be0] kvmalloc_node at ffffffff9c43eba3
 #9 [ffffbd3602893c00] single_open_size at ffffffff9c4f47bc
#10 [ffffbd3602893c38] proc_reg_open at ffffffff9c55ceee
#11 [ffffbd3602893c70] do_dentry_open at ffffffff9c4c803d
#12 [ffffbd3602893ca0] path_openat at ffffffff9c4dc4f6
#13 [ffffbd3602893d78] do_filp_open at ffffffff9c4deadb
#14 [ffffbd3602893ea8] do_sys_open at ffffffff9c4c98ed
#15 [ffffbd3602893f00] new_openat at ffffffffc071b0d5 [fsbackup]
#16 [ffffbd3602893f38] do_syscall_64 at ffffffff9c20432f
#17 [ffffbd3602893f50] entry_SYSCALL_64_after_hwframe at ffffffff9cc000a0
    RIP: 00007f689d49f2b9  RSP: 00007ffefacec700  RFLAGS: 00000293
    RAX: ffffffffffffffda  RBX: 0000556d7906b260  RCX: 00007f689d49f2b9
    RDX: 0000000000000000  RSI: 0000556d783d1293  RDI: 00000000ffffff9c
    RBP: 0000000000000008   R8: 0000000000000000   R9: 0000000000000001
    R10: 0000000000000000  R11: 0000000000000293  R12: 0000556d783d0216
    R13: 0000556d783d0216  R14: 0000000000000001  R15: 0000556d79079210
    ORIG_RAX: 0000000000000101  CS: 0033  SS: 002b
```

- 只输出 panic 运行时堆栈
```shell
crash> bt -p
PID: 965    TASK: ffff9c5a03c2bc00  CPU: 0   COMMAND: "irqbalance"
 #0 [ffffbd36028938a0] machine_kexec at ffffffff9c254a9f
 #1 [ffffbd36028938f8] __crash_kexec at ffffffff9c357af1
 #2 [ffffbd36028939b8] crash_kexec at ffffffff9c3589dd
 #3 [ffffbd36028939d0] oops_end at ffffffff9c22331f
 #4 [ffffbd36028939f0] do_trap at ffffffff9c21fac2
 #5 [ffffbd3602893a30] do_error_trap at ffffffff9c21ffef
 #6 [ffffbd3602893af0] stack_segment at ffffffff9cc0112e
 #7 [ffffbd3602893b78] __kmalloc_node at ffffffff9c49a9d3
 #8 [ffffbd3602893be0] kvmalloc_node at ffffffff9c43eba3
 #9 [ffffbd3602893c00] single_open_size at ffffffff9c4f47bc
#10 [ffffbd3602893c38] proc_reg_open at ffffffff9c55ceee
#11 [ffffbd3602893c70] do_dentry_open at ffffffff9c4c803d
#12 [ffffbd3602893ca0] path_openat at ffffffff9c4dc4f6
#13 [ffffbd3602893d78] do_filp_open at ffffffff9c4deadb
#14 [ffffbd3602893ea8] do_sys_open at ffffffff9c4c98ed
#15 [ffffbd3602893f00] new_openat at ffffffffc071b0d5 [fsbackup]
#16 [ffffbd3602893f38] do_syscall_64 at ffffffff9c20432f
#17 [ffffbd3602893f50] entry_SYSCALL_64_after_hwframe at ffffffff9cc000a0
    RIP: 00007f689d49f2b9  RSP: 00007ffefacec700  RFLAGS: 00000293
    RAX: ffffffffffffffda  RBX: 0000556d7906b260  RCX: 00007f689d49f2b9
    RDX: 0000000000000000  RSI: 0000556d783d1293  RDI: 00000000ffffff9c
    RBP: 0000000000000008   R8: 0000000000000000   R9: 0000000000000001
    R10: 0000000000000000  R11: 0000000000000293  R12: 0000556d783d0216
    R13: 0000556d783d0216  R14: 0000000000000001  R15: 0000556d79079210
    ORIG_RAX: 0000000000000101  CS: 0033  SS: 002b
```
