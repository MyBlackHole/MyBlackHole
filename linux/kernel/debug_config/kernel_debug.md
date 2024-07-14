# Kernel Debugging

```shell

make menuconf
## 配置开启KGDB参数,然后光标选择save,将选项保存到.config文件中
## Kernel hacking --->
##    Compile-time checks and compiler options --->
##        [*] Compile the kernel with debug info
##        [*] Provide GDB scripts for kernel debugging
##    Generic Kernel Debugging Instruments --->
##        [*] KGDB: kernel debugger
## General setup  --->  
##     [*] Configure standard kernel features (expert users)  --->
##         （选中此项，才有/proc/kallsyms接口文件, oops问题，选中此选项即可，子选项可以忽略）
##         [*]   Load all symbols for debugging/ksymoops 
##               [*]   Include all symbols in kallsyms
##               [*]   Do an extra kallsyms pass  
## 

Kernel hacking
--> Compile-time checks and compiler options
    --> [*] Provide GDB scripts for kernel debugging
    --> [*] Enable full Section mismatch analysis
    --> Debug information (Rely on the toolchain's implicit default DWARF version)

CONFIG_KGDB=y
CONFIG_DEBUG_BUGVERBOSE=y
CONFIG_DEBUG_SECTION_MISMATCH=y  # 防止内联
CONFIG_DEBUG_INFO=y
CONFIG_DEBUG_INFO_DWARF_TOOLCHAIN_DEFAULT=y  # 新版本特性
CONFIG_DEBUG_KERNEL=y
CONFIG_FRAME_POINTER=y  # Makefile中选择GCC编译选项
CONFIG_GDB_SCRIPTS=y  # gdb python
CONFIG_EFI_PARTITION=y

<!-- debug 相关 -->
Kernel hacking  --->
  Compile-time checks and compiler options  --->
    [*] Compile the kernel with debug info
    [*]   Provide GDB scripts for kernel debugging

<!-- 关闭内核地址空间布局随机化 -->
Processor type and features  --->
  [ ]   Randomize the address of the kernel image (KASLR)
  <!-- [ ] Randomize the kernel memory sections -->

-> Processor type and features
  -> Build a relocatable kernel
  [ ]   Randomize the address of the kernel image (KASLR)

<!-- 关闭签名验证 -->
-*- Cryptographic API  --->
  Certificates for signature checking  --->
    (debian/canonical-certs.pem) Additional X.509 keys for default system keyring

<!-- 构建内核 -->
make -j$(nproc)
<!-- 编译模块 -->
make modules - j4
<!-- 安装模块 -->
make modules_install -j4

make -j $(nproc)   # 进行编译,此处nproc变量为cpu核心的个数


target remote :1234

<!-- 必须硬断点 -->
hbreak start_kernel
```
