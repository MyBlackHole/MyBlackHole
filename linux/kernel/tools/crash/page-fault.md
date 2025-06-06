# Page Fault


```shell
<!-- linux/arch/x86/include/asm/trap_pf.h -->
Page fault error code bits:
                                                                       
    bit 0 ==	 0: no page found	1: protection fault
    bit 1 ==	 0: read access		1: write access
    bit 2 ==	 0: kernel-mode access	1: user-mode access
    bit 3 ==				1: use of reserved bit detected
    bit 4 ==				1: fault was an instruction fetch
    bit 5 ==				1: protection keys block access
    bit 6 ==				1: shadow stack access fault
    bit 15 ==				1: SGX MMU page-fault
    bit 31 ==				1: fault was due to RMP violation

    位 0 == 0：未找到页面 1：保护错误
    位 1 == 0：读访问 1：写访问
    位 2 == 0：内核模式访问 1：用户模式访问
    位 3 == 1：检测到使用保留位
    位 4 == 1：错误为取指令
    位 5 == 1：保护密钥块访问
    位 6 == 1：影子堆栈访问错误
    位 15 == 1：SGX MMU 页面错误
    位 31 == 1：错误由 RMP 冲突引起

error code 0003 [#1] 表示:
异常由无效的访问权限引起，也就是说被访问的地址存在对应的物理页，但是没有权限访问;
异常由写操作引起;
异常发生在内核态，总结来说就是该异常由于在内核态对没有写权限的地址进行写操作时产生;
而 Oops 中的 [#1] 表示发生 Crash 次数。
```
