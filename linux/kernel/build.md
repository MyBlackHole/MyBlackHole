# build

## config
```shell
make config - 纯文本界面 (最常用的选择)。 
make menuconfig - 基于文本彩色菜单和单选列表。这个选项可以加快开发者开发速度。需要安装ncurses(ncurses-devel)。 
make nconfig - 基于文本的彩色菜单。需要安装curses (libcdk5-dev)。 
make xconfig - QT/X-windows 界面。需要安装QT。 
make gconfig - Gtk/X-windows 界面。需要安装GTK。 
make oldconfig - 纯文本界面，但是其默认的问题是基于已有的本地配置文件。 
make silentoldconfig - 和oldconfig相似，但是不会显示配置文件中已有的问题的回答。 
make olddefconfig -和silentoldconfig相似，但有些问题已经以它们的默认值选择。 
make defconfig - 这个选项将会创建一份以当前系统架构为基础的默认设置文件。 
make ${PLATFORM}defconfig - 创建一份使用arch/$ARCH/configs/${PLATFORM}defconfig中的值的配置文件。 
make allyesconfig - 这个选项将会创建一份尽可能多的问题回答都为‘yes’的配置文件。 
make allmodconfig - 这个选项将会创建一份将尽可能多的内核部分配置为模块的配置文件。

make allnoconfig - 这个选项只会生成内核所必要代码的配置文件。它对尽可能多的问题都回答no。这有时会导致内核无法工作在为编译该内核的硬件上。 
make randconfig - 这个选项会对内核选项随机选择。 
make localmodconfig - 这个选项会根据当前已加载模块列表和系统配置来生成配置文件。 
make localyesconfig - 将所有可装载模块（LKM）都编译进内核(
```

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
## General setup  --->  
##     [*] Configure standard kernel features (expert users)  --->
##         （选中此项，才有/proc/kallsyms接口文件, oops问题，选中此选项即可，子选项可以忽略）
##         [*]   Load all symbols for debugging/ksymoops 
##               [*]   Include all symbols in kallsyms
##               [*]   Do an extra kallsyms pass  
## 

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
