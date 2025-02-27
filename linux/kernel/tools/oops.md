# oops

## 1. insmod 一个 LSM 的 ko 模块，内核打印如下日志：

```shell
[  415.746844] BUG: unable to handle kernel paging request at ffffffffaa6f0210
[  415.746846] PGD 3fc0e067 P4D 3fc0e067 PUD 3fc0f063 PMD 34367063 PTE 800000003faf0061
[  415.746849] Oops: 0003 [#1] SMP PTI
[  415.746851] CPU: 0 PID: 8366 Comm: insmod Tainted: G           OE     4.19.82-wwh #1
[  415.746852] Hardware name: VMware, Inc. VMware Virtual Platform/440BX Desktop Reference Platform, BIOS 6.00 07/29/2019
[  415.746856] RIP: 0010:init_lsm_hooks+0x1cd/0x1f0 [xxx_security]
[  415.746857] Code: 00 00 48 8b 41 10 48 89 71 20 48 8b 10 48 85 d2 75 05 eb 25 48 89 c2 48 8b 02 48 85 c0 75 f5 48 c7 01 00 00 00 00 48 89 51 08 <48> 89 0a 48 83 c1 28 48 39 f9 75 cc 31 c0 c3 48 c7 01 00 00 00 00
[  415.746858] RSP: 0018:ffffa763c332fcb0 EFLAGS: 00010246
[  415.746859] RAX: 0000000000000000 RBX: 0000000000000000 RCX: ffffffffc0ae2020
[  415.746859] RDX: ffffffffaa6f0210 RSI: ffffffffc0adfaa9 RDI: ffffffffc0ae2458
[  415.746860] RBP: ffffffffc0ada000 R08: 000000000000006d R09: ffffffffaa5c11f3
[  415.746860] R10: ffffffffaa63c310 R11: ffffffffaa63c6a0 R12: ffffffffc0ae2fc0
[  415.746861] R13: ffffffffc0ae2e58 R14: ffffa763c332fe98 R15: ffffffffc0ae2e40
[  415.746862] FS:  00007fdf33491200(0000) GS:ffff9ac97bc00000(0000) knlGS:0000000000000000
[  415.746863] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  415.746863] CR2: ffffffffaa6f0210 CR3: 0000000077222004 CR4: 00000000003606f0
[  415.746880] Call Trace:
[  415.746883]  init_module+0x34/0xc0 [xxx_security]
[  415.746886]  do_one_initcall+0x46/0x1c3
[  415.746889]  ? _cond_resched+0x15/0x30
[  415.746890]  ? kmem_cache_alloc_trace+0x155/0x1d0
[  415.746892]  do_init_module+0x5a/0x210
[  415.746894]  load_module+0x215b/0x2390
[  415.746897]  ? __do_sys_finit_module+0xa8/0x110
[  415.746898]  __do_sys_finit_module+0xa8/0x110
[  415.746900]  do_syscall_64+0x55/0xf0
[  415.746901]  entry_SYSCALL_64_after_hwframe+0x44/0xa9
[  415.746910] RIP: 0033:0x7fdf335abf59

```

## 2. Oops 访存出错地址分析

BUG: unable to handle kernel paging request at ffffffffaa6f0210

Unable to handle kernel paging request at virtual address 是内存访问异常的错误，原因通常有以下三种：
virtual address 为 0x00000000 时，说明使用了空指针；
virtual address 没有越出内核地址空间范围，说明指针指向的内存受到某种限制；
除此以外就是指针越出内核地址空间范围；
以上日志中的出错地址 ffffffffaa6f0210 在内核地址空间范围，可以判断为试图篡改受限制内存导致报错；例如给一个声明为 const 的变量赋值就会出现这种错误。

## 3. 引起 Oops 的内核代码位置初步分析

接下来看看日志中的 RIP 信息：

RIP: 0010:init_lsm_hooks+0x1cd/0x1f0 [xxx_security]
通常，我们在这种情况下从 Oops 收集的最有用的信息是 EIP 和错误的调用地址。对于 64bit 用户来说，你可能需要查看 RIP, EIP/RIP 通常标识了问题发生的现场。在这个例子中我们可以看到 Oops 中 RIP 是在 init_lsm_hooks 的 0x1cd 字节的位置，而 init_lsm_hooks 占用 0x1f0 字节的大小，它给出了一个很有用的信息，告诉我们去哪里寻找出错的代码。下文会详细介绍准确找出出错代码的过程。

而 Oops 日志中的 Code 行，会把导致 Oops 的第一条指令，也就是 RIP 的值的第一个字节，用尖括号 <> 括起来。 如:

Code: 00 00 48 8b 41 10 48 89 71 20 48 8b 10 48 85 d2 75 05 eb 25 48 89 c2 48 8b 02 48 85 c0 75 f5 48 c7 01 00 00 00 00 48 89 51 08 <48> 89 0a 48 83 c1 28 48 39 f9 75 cc 31 c0 c3 48 c7 01 00 00 00 00

<48> 即是。
这种 Code 行，在没有自己编译的 vmlinux 时又想定位出错的代码行，可以利用。但是要注意 cpu 的架构问题，有些架构的（例如常见的x86）指令是不等长的;

## 4. Oops Error Code 解析

再看看 Error Code 行：

Oops: 0003 [#1] SMP PTI
其中 0003 为 error code，当异常发生时，由硬件压入栈中。可以通过这个看出 Oops 发生的大致原因。

对于 x86 架构来说，error code 具体定义如下：

Page fault error code bits:
bit 0 == 0: no page found 1: protection fault
bit 1 == 0: read access 1: write access
bit 2 == 0: kernel-mode access 1: user-mode access
bit 3 == 1: use of reserved bit detected
bit 4 == 1: fault was an instruction fetch
常用低 3 位，具体含义为：

如果第 0 位被清 0，则异常是由一个不存在的页所引起的；否则是由无效的访问权限引起的。
如果第 1 位被清 0，则异常由读访问或者执行访问所引起；否则异常由写访问引起。
如果第 2 位被清 0，则异常发生在内核态；否则异常发生在用户态。
所以，上述样例中的 error code 0003 表示:

异常由无效的访问权限引起，也就是说被访问的地址存在对应的物理页，但是没有权限访问;
异常由写操作引起;
异常发生在内核态，总结来说就是该异常由于在内核态对没有写权限的地址进行写操作时产生;
而 Oops 中的 [#1] 表示发生 Crash 次数。


## 5. 通过 gdb 定位出错的内核代码文件和行号


接下来再通过 gdb 调试做进一步动态的分析，以便确定出错的代码文件和行号。

首先需要通过 add-symbol-file 命令将符号文件添加到调试器。符号文件即通过 insmod 命令插入 LSM 模块出错时用的内核模块的 .o 文件，即日志中出现的 xxx_security。

insmod 命令:

[  415.746851] CPU: 0 PID: 8366 Comm: insmod Tainted: G           OE     4.19.82-wwh #1
xxx_security 模块:

[  415.746883]  init_module+0x34/0xc0 [xxx_security]
add-symbol-file 命令有两个参数：

第一个参数是 xxx_security.o
第二个参数是该模块代码正文区域的地址
该地址通过如下方式获得:

$ sudo cat /sys/module/xxx_security/sections/.text
0xffffffffc0ada000
接着通过 gdb 来调试 xxx_security.ko：

(gdb) add-symbol-file xxx_security.o 0xffffffffc0ada000
add symbol table from file "xxx_security.o" at
    .text_addr = 0xffffffffc0ada000
(y or n) y
Reading symbols from xxx_security.o...done.
上文已经根据 RIP 行可以得到报错函数名以及偏移：

RIP: 0010:init_lsm_hooks+0x1cd/0x1f0 [xxx_security]
接着就是反汇编 init_lsm_hooks 函数如下:

(gdb) disassemble init_lsm_hooks
Dump of assembler code for function init_lsm_hooks:
Address range 0x150 to 0x33c:
   0x0000000000000150 <+0>:	callq  0x155 <init_lsm_hooks+5>
   0x0000000000000312 <+450>:	movq   $0x0,(%rcx)
   0x0000000000000319 <+457>:	mov    %rdx,0x8(%rcx)
   0x000000000000031d <+461>:	mov    %rcx,(%rdx)
   0x0000000000000320 <+464>:	add    $0x28,%rcx
   0x0000000000000324 <+468>:	cmp    %rdi,%rcx
从上可以看出 init_lsm_hooks 函数的起始地址是 0x150，出错所在的偏移是 0x1cd。

0x150+0x1cd=0x31d，那么如何通过这个地址对应到 .c 中具体某一行呢？

(gdb) l *0x000000000000031d
0x31d is in init_lsm_hooks (./include/linux/compiler.h:220).
215	{
216		switch (size) {
217		case 1: *(volatile __u8 *)p = *(__u8 *)res; break;
218		case 2: *(volatile __u16 *)p = *(__u16 *)res; break;
219		case 4: *(volatile __u32 *)p = *(__u32 *)res; break;
220		case 8: *(volatile __u64 *)p = *(__u64 *)res; break;
221		default:
222			barrier();
223			__builtin_memcpy((void *)p, (const void *)res, size);
224			barrier();
从调试信息可以看出，出错位置为：

./include/linux/compiler.h:220 case 8: *(volatile __u64 *)p = *(__u64 *)res; break`
打开该文件以及所在行可以确认：

$ vim include/linux/compiler.h +220
static __always_inline void __write_once_size(volatile void *p, void *res, int size)
{
        switch (size) {
        case 1: *(volatile __u8 *)p = *(__u8 *)res; break;
        case 2: *(volatile __u16 *)p = *(__u16 *)res; break;
        case 4: *(volatile __u32 *)p = *(__u32 *)res; break;
        case 8: *(volatile __u64 *)p = *(__u64 *)res; break;
        default:
                barrier();
                __builtin_memcpy((void *)p, (const void *)res, size);
                barrier();
        }
}
当然，也可以通过 set listsize 20 之后看到具体的 __write_once_size 函数。该函数为 __always_inline 的，编译时被嵌入到了 init_lsm_hooks 中。

以上过程也可以通过其他方式快速定位，可进一步阅读：

如何快速定位 Linux Panic 出错的代码行
bugfix: 消除 qemu/raspi3 启动过程的一堆警告


## 6. 出错代码详细分析

上述 __write_once_size 接口是内核中所有双链表操作最终进入的函数。

在出错的 init_lsm_hooks 函数中通过调用 hlist_add_tail_rcu 调用 WRITE_ONCE, 从而进入了 __write_once_size, 接下来看看这个接口的实现:

#define WRITE_ONCE(x, val) \
({							\
	union { typeof(x) __val; char __c[1]; } __u =	\
		{ .__val = (__force typeof(x)) (val) }; \
	__write_once_size(&(x), __u.__c, sizeof(x));	\
	__u.__val;					\
})
WRITE_ONCE() 用于向变量对应的内存写入值。x 对应变量，val 对应写入的值。函数首先定义并初始化一个联合体，使 __u.__val 的值为参数 val， 然后调用 __write_once_size() 函数将数据写入到内存中。

此时变量 x 所在地址 &(x) 为需要写入 val 的内存虚拟地址 ffffffffaa6f0210, 可以通过 Crash 查看该虚拟地址各项属性:

crash> vtop ffffffffaa6f0210
VIRTUAL           PHYSICAL
ffffffffaa6f0210  3faf0210
PGD DIRECTORY: ffffffffaa80a000
PAGE DIRECTORY: 3fc0e067
   PUD: 3fc0eff0 => 3fc0f063
   PMD: 3fc0fa98 => 34367063
   PTE: 34367780 => 800000003faf0061
  PAGE: 3faf0000
      PTE         PHYSICAL  FLAGS
800000003faf0061  3faf0000  (PRESENT|ACCESSED|DIRTY|NX)
      PAGE       PHYSICAL      MAPPING       INDEX CNT FLAGS
ffffe02200febc00 3faf0000         0            0    1 ffffc000000800 reserved
可以看到该虚拟地址所在 PAGE 的 PTE 页表项内容，从 FLAGS 可以看出该 PAGE 不具备 R/W 属性。因而可以确认，是由于尝试向只读地址空间写数据，从而导致内核报错。

这里稍作补充，上述 PUD/PMD/PTE 信息跟第一节日志中的第二行信息一致：

PGD 3fc0e067 P4D 3fc0e067 PUD 3fc0f063 PMD 34367063 PTE 800000003faf0061
知道是在调用 hlist_add_tail_rcu 接口出错后也可以通过查看符号表得到所要写入的地址空间是否是只读的，在内核的安全模块中默认配置了 apparmor，当以 ko 的模式挂载外部的 LSM 钩子时，都需要以双链表方式挂在 apparmor 的 LSM 尾部:

$ sudo cat /proc/kallsyms | grep apparmor_hooks
ffffffffaa6eff40 r apparmor_hooks
可以看出此时 apparmor_hooks 所在内存空间是只读的。


## 7. 问题解决方案

进一步查看到 apparmor_hooks 的定义如下:

static struct security_hook_list apparmor_hooks[] __lsm_ro_after_init
__lsm_ro_after_init 表示 LSM 架构在完成初始化后所在的内存空间会成只读。

因此我们提出两种解决方案：

解决方案1: 将 __lsm_ro_after_init 标志位去掉，让该 PAGE 可读可写，这样修改虽然可行，但不确定是否会引入其他问题。

解决方案2: 可以通过内核配置添加 SECURITY_WRITABLE_HOOKS 选项，同样会将该 PAGE 配置成可读可写。


> https://tinylab.org/lsm-crash/
