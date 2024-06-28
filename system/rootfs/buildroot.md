# Buildroot

```shell
git://git.buildroot.net/buildroot
```

- build
```shell
make menuconfig
## Target options --->
##     Target Architecture --->
##         (X) x86_64
## Toolchain --->
##     Toolchain type --->
##         (X) Buildroot toolchain
##     C library --->
##         (X) glibc
##     Kernel Headers
##         (X) Linux 5.15.x kernel headers
##     [*] Install glibc utilities
##     [*] Enable C++ support
##     [*] Build cross gdb for the host
##     [*] TUI support
##     [*] Python support
##     [*] Simulator support
## Filesystem images --->
##     [*] ext2/3/4 root filesystem
##         ext2/3/4 variant --->
##             (X) ext4
## Bootloaders --->
##     [*] grub2
##     [ ] i386-pc
##     [ ] i386-efi
##     [*] x86-64-efi
##     (boot linux ext4 fat squash4 part_msdos part_gpt normal efi_gop) builtin modules

export PATH=/usr/bin:/bin

make -j $(nproc) # 开始编译

## 编译完成后，在output/images/目录下可以找到编译好的镜像文件。
```

## error

- Makefile.legacy:9: *** "You have legacy configuration in your .config! Please check your configuration.".  Stop.
```shell
删除.config文件，重新配置
```

- 
You seem to have the current working directory in your
PATH environment variable. This doesn't work.
make: *** [support/dependencies/dependencies.mk:27: dependencies] Error 1
```shell
删除PATH环境变量中的当前工作目录

export PATH=/usr/bin:/bin

<!-- unset LD_LIBRARY_PATH -->
```

- ....stamp_host_installed] Error 1
```shell
make clean world
```
