# Syscall Table

[连接](https://filippo.io/linux-syscall-table/)

## sys_call_table 获取方式

- 编译内核时，在内核源码的顶层目录下生成 `System.map` 文件。
❯ cat System.map|grep sys_call_table
ffffffff820001a0 D sys_call_table
ffffffff82000e60 D ia32_sys_call_table

- kallsyms_lookup_name

```c
#include <linux/kallsyms.h>
void *sys_call_table = (void *)kallsyms_lookup_name("sys_call_table");
```

- 直接从 `kernel_symbols` 结构体中获取, 设置 sys_call_table 数组为可写状态
```c
#include <linux/kernel.h>
void *sys_call_table = (void *)kernel_symbols.sys_call_table;
```

第一种方法: 将 cr0 寄存器的第 16 位设置为零
cr0 控制寄存器的第 16 位是写保护位，若设置为零，则允许超级权限往内核中写入数据。
这样我们可以在修改 sys_call_table 数组的值前，将 cr0 寄存器的第 16 位清零，使其可以修改 sys_call_table 数组的内容。
当修改完后，又将那一位复原即可
```c
/*
 * 设置cr0寄存器的第16位为0
 */
unsigned int clear_and_return_cr0(void)
{
    unsigned int cr0 = 0;
    unsigned int ret;

    /* 将cr0寄存器的值移动到rax寄存器中，同时输出到cr0变量中 */
    asm volatile ("movq %%cr0, %%rax" : "=a"(cr0));

    ret = cr0;
    cr0 &= 0xfffeffff;  /* 将cr0变量值中的第16位清0，将修改后的值写入cr0寄存器 */

    /* 读取cr0的值到rax寄存器，再将rax寄存器的值放入cr0中 */
    asm volatile ("movq %%rax, %%cr0" :: "a"(cr0));

    return ret;
}

/*
 * 还原cr0寄存器的值为val
 */
void setback_cr0(unsigned int val)
{
    asm volatile ("movq %%rax, %%cr0" :: "a"(val));
}
```

第二种方法: 设置虚拟地址对应页表项的读写属性
由于 x86 CPU 的内存保护机制是通过虚拟内存页表来实现的（可以参考这篇文章：漫谈内存映射），
所以我们只需要把 sys_call_table 数组的虚拟内存页表项中的保护标志位清空即可

```c
/*
 * 把虚拟内存地址设置为可写
 */
int make_rw(unsigned long address)
{
    unsigned int level;

    //查找虚拟地址所在的页表地址
    pte_t *pte = lookup_address(address, &level);

    if (pte->pte & ~_PAGE_RW)  //设置页表读写属性
        pte->pte |=  _PAGE_RW;

    return 0;
}

/*
 * 把虚拟内存地址设置为只读
 */
int make_ro(unsigned long address)
{
    unsigned int level;

    pte_t *pte = lookup_address(address, &level);
    pte->pte &= ~_PAGE_RW;  //设置只读属性

    return 0;
}
```

## 函数

查找方法：find_sys_call_table 通过遍历内存查找特定模式找到系统调用表，而 kallsyms_lookup_name 通过符号名称直接查找地址。
适用场景：find_sys_call_table 适用于符号信息不可用或者不支持 kallsyms_lookup_name 的情况，而 kallsyms_lookup_name 适用于开发和调试，需要通过符号名称快速查找地址的情况。
实现复杂度：find_sys_call_table 通常实现较复杂，需要了解内核内存布局和系统调用表特征；kallsyms_lookup_name 使用方便，只需提供符号名称即可。

### find_sys_call_table

- 作用
find_sys_call_table 是一种通过遍历内核内存或者其他方式来手动查找系统调用表地址的技术。这种方法通常在某些情况下使用，比如系统没有启用 kallsyms_lookup_name 或者内核版本不支持直接符号查找。

- 使用场景
内核版本不支持 kallsyms_lookup_name：在某些老版本内核中，kallsyms_lookup_name 可能不可用或者被禁用。
无符号信息的内核：某些内核可能没有编译符号信息，这时无法通过符号名直接查找地址。
调试和分析工具：在某些情况下，调试工具可能需要手动查找系统调用表。

- 实现方法
find_sys_call_table 通常通过扫描内存中的特定模式来找到系统调用表。以下是一个简化的示例，展示了如何通过遍历内存来找到 sys_call_table
这个示例通过遍历从 PAGE_OFFSET 开始的内核内存地址空间，查找包含 sys_close 函数地址的位置，来确定系统调用表的位置
```c
unsigned long **find_sys_call_table(void) {
    unsigned long offset;
    unsigned long **sct;

    for (offset = PAGE_OFFSET; offset < ULLONG_MAX; offset += sizeof(void *)) {
        sct = (unsigned long **)offset;

        if (sct[__NR_close] == (unsigned long *) sys_close) {
            return sct;
        }
    }
    return NULL;
}
```


### kallsyms_lookup_name

- 作用
kallsyms_lookup_name 是内核提供的一个函数，用于通过符号名称查找其地址。这在调试和开发内核模块时非常方便，能够直接获取符号（如函数、变量等）的地址。

- 使用场景
内核开发和调试：在开发内核模块时，通过符号名称快速查找函数或变量的地址。
符号查找：需要查找特定符号的地址时，如 sys_call_table 的地址。

- 使用方法
使用 kallsyms_lookup_name 需要确保内核配置启用了 CONFIG_KALLSYMS 和 CONFIG_KALLSYMS_ALL，并且函数是GPL导出的

```c
#include <linux/kallsyms.h>
#include <linux/module.h>

static int __init init_module_func(void) {
    unsigned long address;

    address = kallsyms_lookup_name("sys_call_table");
    if (address) {
        printk(KERN_INFO "sys_call_table address: %lx\n", address);
    } else {
        printk(KERN_ERR "sys_call_table not found\n");
    }
    return 0;
}

static void __exit cleanup_module_func(void) {
    printk(KERN_INFO "Module exiting\n");
}

module_init(init_module_func);
module_exit(cleanup_module_func);

MODULE_LICENSE("GPL");
```

