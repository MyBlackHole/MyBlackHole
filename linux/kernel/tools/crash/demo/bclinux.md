# bclinux

```shell
      KERNEL: /usr/lib/debug/usr/lib/modules/4.19.90-2107.6.0.0251.43.oe1.bclinux.x86_64/vmlinux
    DUMPFILE: vmcore  [PARTIAL DUMP]
        CPUS: 4
        DATE: Thu May 29 14:51:29 2025
      UPTIME: 02:30:16
LOAD AVERAGE: 0.08, 0.08, 0.07
       TASKS: 216
    NODENAME: 67.192
     RELEASE: 4.19.90-2107.6.0.0251.43.oe1.bclinux.x86_64
     VERSION: #1 SMP Tue Jul 2 13:51:22 CST 2024
     MACHINE: x86_64  (2199 Mhz)
      MEMORY: 16 GB
       PANIC: "stack segment: 0000 [#1] SMP NOPTI"
         PID: 965
     COMMAND: "irqbalance"
        TASK: ffff9c5a03c2bc00  [THREAD_INFO: ffff9c5a03c2bc00]
         CPU: 0
       STATE: TASK_RUNNING (PANIC)



[ 9016.879381] stack segment: 0000 [#1] SMP NOPTI
[ 9016.879404] CPU: 0 PID: 965 Comm: irqbalance Kdump: loaded Tainted: G           OE     4.19.90-2107.6.0.0251.43.oe1.bclinux.x86_64 #1
[ 9016.879447] Hardware name: VMware, Inc. VMware Virtual Platform/440BX Desktop Reference Platform, BIOS 6.00 11/12/2020
[ 9016.879480] RIP: 0010:__kmalloc_node+0x123/0x280
[ 9016.879494] Code: 0e 41 f6 47 0b 04 4d 89 f9 0f 84 6b ff ff ff 4c 89 ff e8 60 d3 01 00 49 89 c1 e9 5b ff ff ff 41 8b 41 20 49 8b 39 48 8d 4a 01 <48> 8b 5c 05 00 48 89 e8 65 48 0f c7 0f 0f 94 c0 84 c0 0f 84 41 ff
[ 9016.879541] RSP: 0018:ffffbd3602893ba8 EFLAGS: 00010246
[ 9016.879576] RAX: 0000000000000000 RBX: 00000000007012c0 RCX: 0000000000015a88
[ 9016.879638] RDX: 0000000000015a87 RSI: 00000000007012c0 RDI: 00000000000271c0
[ 9016.880299] RBP: 6d69742f72656761 R08: ffff9c5d2dc271c0 R09: ffff9c5a00006680
[ 9016.880908] R10: ffffffff9d667a60 R11: 0000000000000000 R12: 00000000007012c0
[ 9016.881488] R13: 0000000000001150 R14: 00000000ffffffff R15: ffff9c5a00006680
[ 9016.882102] FS:  00007f689d31b300(0000) GS:ffff9c5d2dc00000(0000) knlGS:0000000000000000
[ 9016.882725] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[ 9016.883349] CR2: 00007fb0bde9f040 CR3: 0000000103892003 CR4: 00000000007606f0
[ 9016.884018] PKRU: 55555554
[ 9016.884750] Call Trace:
[ 9016.885507]  ? kvmalloc_node+0x43/0x70
[ 9016.886178]  kvmalloc_node+0x43/0x70
[ 9016.886950]  ? get_idle_time+0x60/0x60
[ 9016.887837]  single_open_size+0x3c/0xb0
[ 9016.888594]  proc_reg_open+0x6e/0x130
[ 9016.889281]  ? proc_alloc_inode+0x60/0x60
[ 9016.889967]  do_dentry_open+0x1ed/0x390
[ 9016.890637]  path_openat+0x2e6/0x1540
[ 9016.891292]  ? vsnprintf+0xfe/0x4d0
[ 9016.891954]  ? seq_vprintf+0x30/0x50
[ 9016.892595]  do_filp_open+0x9b/0x110
[ 9016.893245]  ? prepend_path+0xfc/0x330
[ 9016.893882]  ? page_counter_try_charge+0x57/0xc0
[ 9016.894522]  ? files_cgroup_alloc_fd+0x58/0x70
[ 9016.895180]  ? __alloc_fd+0xda/0x180
[ 9016.895841]  ? do_sys_open+0x1bd/0x250
[ 9016.896460]  do_sys_open+0x1bd/0x250
[ 9016.897106]  new_openat+0x85/0x110 [xxxxxxxx]
[ 9016.897726]  ? do_syscall_64+0x5f/0x240
[ 9016.898328]  ? entry_SYSCALL_64_after_hwframe+0x5c/0xc1
[ 9016.898940] Modules linked in: fsbackup(OE) vmw_vsock_vmci_transport vsock rfkill intel_rapl_msr intel_rapl_common isst_if_mbox_msr isst_if_common nfit libnvdimm vmw_balloon crct10dif_pclmul vmwgfx ttm crc32_pclmul ghash_clmulni_intel drm_kms_helper syscopyarea sysfillrect sysimgblt fb_sys_fops drm sg intel_rapl_perf pcspkr joydev vmw_vmci i2c_piix4 ip_tables ext4 mbcache jbd2 sr_mod cdrom sd_mod ata_generic ahci crc32c_intel serio_raw libahci vmxnet3 ata_piix vmw_pvscsi libata dm_mirror dm_region_hash dm_log dm_mod

RIP: 0010:__kmalloc_node+0x123/0x280
RIP 是在 __kmalloc_node 的 0x123 字节的位置，而 __kmalloc_node 占用 0x280 字节的大小

<!-- 第一个参数：RDI -->
<!-- 第二个参数：RSI -->
<!-- 第三个参数：RDX -->

crash> dis kvmalloc_node
0xffffffff9c43eb60 <kvmalloc_node>:     nopl   0x0(%rax,%rax,1) [FTRACE NOP]
0xffffffff9c43eb65 <kvmalloc_node+5>:   mov    %esi,%eax
0xffffffff9c43eb67 <kvmalloc_node+7>:   and    $0x6000c0,%eax
0xffffffff9c43eb6c <kvmalloc_node+12>:  cmp    $0x6000c0,%eax
0xffffffff9c43eb71 <kvmalloc_node+17>:  je     0xffffffff9c43eb78 <kvmalloc_node+24>
0xffffffff9c43eb73 <kvmalloc_node+19>:  jmpq   0xffffffff9c49a8b0 <__kmalloc_node>
0xffffffff9c43eb78 <kvmalloc_node+24>:  cmp    $0x1000,%rdi
0xffffffff9c43eb7f <kvmalloc_node+31>:  jbe    0xffffffff9c43eb73 <kvmalloc_node+19>
0xffffffff9c43eb81 <kvmalloc_node+33>:  push   %r12
0xffffffff9c43eb83 <kvmalloc_node+35>:  mov    %esi,%eax
0xffffffff9c43eb85 <kvmalloc_node+37>:  push   %rbp
0xffffffff9c43eb86 <kvmalloc_node+38>:  push   %rbx
0xffffffff9c43eb87 <kvmalloc_node+39>:  mov    %esi,%ebx
0xffffffff9c43eb89 <kvmalloc_node+41>:  or     $0x12,%ah
0xffffffff9c43eb8c <kvmalloc_node+44>:  or     $0x200,%esi
0xffffffff9c43eb92 <kvmalloc_node+50>:  test   $0x4,%bh
0xffffffff9c43eb95 <kvmalloc_node+53>:  mov    %edx,%r12d
0xffffffff9c43eb98 <kvmalloc_node+56>:  cmove  %eax,%esi
0xffffffff9c43eb9b <kvmalloc_node+59>:  mov    %rdi,%rbp
0xffffffff9c43eb9e <kvmalloc_node+62>:  callq  0xffffffff9c49a8b0 <__kmalloc_node>
0xffffffff9c43eba3 <kvmalloc_node+67>:  test   %rax,%rax
0xffffffff9c43eba6 <kvmalloc_node+70>:  jne    0xffffffff9c43ebbe <kvmalloc_node+94>
0xffffffff9c43eba8 <kvmalloc_node+72>:  mov    0x18(%rsp),%rcx
0xffffffff9c43ebad <kvmalloc_node+77>:  mov    %ebx,%edx
0xffffffff9c43ebaf <kvmalloc_node+79>:  mov    %r12d,%esi
0xffffffff9c43ebb2 <kvmalloc_node+82>:  pop    %rbx
0xffffffff9c43ebb3 <kvmalloc_node+83>:  mov    %rbp,%rdi
0xffffffff9c43ebb6 <kvmalloc_node+86>:  pop    %rbp
0xffffffff9c43ebb7 <kvmalloc_node+87>:  pop    %r12
0xffffffff9c43ebb9 <kvmalloc_node+89>:  jmpq   0xffffffff9c4737c0 <__vmalloc_node_flags_caller>
0xffffffff9c43ebbe <kvmalloc_node+94>:  pop    %rbx
0xffffffff9c43ebbf <kvmalloc_node+95>:  pop    %rbp
0xffffffff9c43ebc0 <kvmalloc_node+96>:  pop    %r12
0xffffffff9c43ebc2 <kvmalloc_node+98>:  retq
0xffffffff9c43ebc3 <kvmalloc_node+99>:  data32 nopw %cs:0x0(%rax,%rax,1)
0xffffffff9c43ebce <kvmalloc_node+110>: xchg   %ax,%ax


crash> bt -f
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

crash> dis __kmalloc_node+0x123
0xffffffff9c49a9d3 <__kmalloc_node+291>:        mov    0x0(%rbp,%rax,1),%rbx
```
