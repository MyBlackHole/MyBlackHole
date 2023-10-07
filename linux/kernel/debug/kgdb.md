# kgdb

- 代码位置
kernel/debug/debug_core.c

CONFIG_KGDB
配置CONFIG_KGDB=y，以启用kgdb功能。

CONFIG_GDB_SCRIPTS
配置CONFIG_GDB_SCRIPTS=y，这样在内核编译时会在根目录生成vmlinux-gdb.py文件，这个python脚本需要在gdb调试时加载，以此提供一些供gdb调试内核的扩展命令，对于调试外部ko文件带来极大的方便。

CONFIG_DEBUG_INFO
配置CONFIG_DEBUG_INFO=y，gdb调试内核需要从vmlinx中加载相关符号信息，但默认的内核编译选项没有-g选项，因此该elf文件将不含有调试信息，调试时看不到源码，仅能做汇编级别的调试跟踪，因此需要配置该选项，打开调试信息（打开后会增大内核文件）。

CONFIG_DEBUG_INFO_DWARF4
配置CONFIG_DEBUG_INFO_DWARF4=y，如果不启用这个选项内核调试信息将不包含DWARF4信息，只能进行一些backtarce的查看，而无法debug变量，打开该选项可以使用（打开后会进一步增大内核文件）。

CONFIG_STRICT_KERNEL_RWX/CONFIG_STRICT_MODULE_RWX
配置CONFIG_STRICT_KERNEL_RWX/CONFIG_STRICT_MODULE_RWX选项关闭，这个选项的存在会对内核代码段进行写保护，因此在使用kgdb时如果进行添加断点操作，目标系统如果不支持硬件断点机制，就必须改写内核代码段来使用断点指令触发软件断点达到目的，而该选项的存在会导致断点指令无法写入，因此需要关闭（关闭后会增加内核所在内存被破坏风险）

```shell
CONFIG_DEBUG_INFO=y
CONFIG_FRAME_POINTER=y
CONFIG_MAGIC_SYSRQ=y
CONFIG_MAGIC_SYSRQ_SERIAL=y
CONFIG_KGDB_SERIAL_CONSOLE=y
CONFIG_KGDB_KDB=y
CONFIG_KGDB=y


- bootargs = "root=/dev/vda2 rw console=ttyS0 earlycon=sbi";
+ bootargs = "root=/dev/vda2 rw console=ttyS0 earlycon=sbi nokaslr kgdboc=ttyS1,115200 kgdbwait";
```


![[imgs/Pasted image 20231005103745.png]]

- config
```shell
    Kernel hacking--->
           -*- Magic SysRq key
           [*] Kernel debugging
           [*] Compile the kernel with debug info
           [*] KGDB: kernel debugging with remote gdb --->
                  <*> KGDB: use kgdb over the serial console
```

- 进入 kdb
```shell
echo g > /proc/sysrq-trigger
```

- 调试内核启动流程
```shell
<!-- 启动命令行参数中添加以下选项 -->
kgdbwait
```
