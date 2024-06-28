# build

```shell
/home/black/aio-data/linux-4.19.315
cd /home/black/aio-data && wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.315.tar.xz
tar -xf linux-4.19.315.tar.xz # 解压源码
<!-- #安装编译环境和依赖库 -->
<!-- paru -S libncurses-dev flex bison bc openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf gcc make gnu-standards libtool gettext -->

cd linux-4.19.315 # 进入源码目录
# 生成基于文本界面的内核配置菜单,该操作依赖ncurses库
# 当出现’Your display is too small to run Menuconfig!‘提示时,将终端放大重新执行这条命令即可
make menuconf
## 配置开启KGDB参数,然后光标选择save,将选项保存到.config文件中
## Kernel hacking --->
##    Compile-time checks and compiler options --->
##        [*] Compile the kernel with debug info
##        [*] Provide GDB scripts for kernel debugging
##    Generic Kernel Debugging Instruments --->
##        [*] KGDB: kernel debugger
make -j $(nproc)   # 进行编译,此处nproc变量为cpu核心的个数


❯ ls -alh arch/x86_64/boot/bzImage
Permissions Size User  Date Modified Name
lrwxrwxrwx     - black 11 Jun 14:54   arch/x86_64/boot/bzImage -> ../../x86/boot/bzImage
```


- 内核编译
```shell
cd linux
make menuconfig # 配置内核

<!-- debug 相关 -->
Kernel hacking  --->
  Compile-time checks and compiler options  --->
    [*] Compile the kernel with debug info
    [*]   Provide GDB scripts for kernel debugging

<!-- 关闭内核地址空间布局随机化 -->
Processor type and features  --->
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
```
