# Kernel Debugging

```shell
<!-- https://www.kernel.org/doc/html/latest/translations/zh_CN/dev-tools/gdb-kernel-debugging.html -->
make menuconf

CONFIG_FRAME_POINTER=y  # Makefile中选择GCC编译选项
CONFIG_GDB_SCRIPTS=y  # gdb python

<!-- debug 相关 -->
Kernel hacking  --->
  Compile-time checks and compiler options  --->
    [*] Compile the kernel with debug info
    [*]   Provide GDB scripts for kernel debugging

<!-- 关闭内核地址空间布局随机化 -->
Processor type and features  --->
  [ ]   Randomize the address of the kernel image (KASLR)
  <!-- [ ] Randomize the kernel memory sections -->

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

gdb vmlinux
target remote :1234

<!-- 必须硬断点 -->
hbreak start_kernel
```
