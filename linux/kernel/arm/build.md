# build

```shell
# 对于 arm64
make defconfig ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu-
# make menuconfig ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu-
make -j8 ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu-

# 对于 riscv
make defconfig ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu-
# make menuconfig ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu-
make -j8 ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu-
```

## 交叉编译

[来源](https://blog.51cto.com/u_14230/7350171)
```shell
paru -S arm-linux-gnueabihf-linaro-bin
<!--# 配置交叉编译环境-->
<!--echo "CROSS_COMPILE=/usr/bin/arm-linux-gnueabihf-" >> ~/.bashrc-->
<!--source ~/.bashrc-->

# 编译
<!--make ARCH=arm imx_v7_defconfig-->
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-
make ARCH=arm64 CROSS_COMPILE=arm-linux-gnueabihf- 

# 安装
sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- modules_install


paru -S aarch64-linux-gnu-gcc


# busybox 

Target options->AArch64(little endian)
Target options-> Target Architecture Variant (cortex-A57)
<!--Toolchain->Toolchain type 选择“External toolchain”:-->
<!--Toolchain->Toolchain选择"Arm AArch64 2020.11”:-->
System configuration-> Enable root login with password (1234)
<!--`ttyAMA0` 通常用于 Raspberry Pi 等某些 ARM 设备上的默认串口设备。而 `ttyS0` 则更常见于通用的 x86 架构设备或其他嵌入式系统。-->
System configuration-> Run a getty (login prompt) after boot->TTY port设置为"ttyAMA0"
Target packages->选择Show packages that are also provided by busybox
Target packages-> Debugging, profiling and benchmark (可以选择自己感兴趣的工具，这里选择strace)

make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-


# linux
<!--基础配置-->
CONFIG_CMDLINE="console=ttyAMA0"
<!--CONFIG_INITRAMFS_SOURCE="/home/tom/hdd/linux-qemu/repos/buildroot-2021.08/output/images/rootfs.cpio" (不启用 initramfs)-->
CONFIG_DEBUG_INFO=y
<!--Built-in initramfs compression mode时，因为我们编译initramfs文件系统时，没有选择压缩模式-->
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig

<!--make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- O=build menconfig-->
<!--# O=build: 编译输出到 build 目录-->

# 编译
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- -j8

<!--qemu-system-aarch64 \-->
<!--    -machine virt \-->
<!--    -cpu cortex-a57 \-->
<!--    -machine type=virt \-->
<!--    -nographic -smp 1 \-->
<!--    -m 2048 \-->
<!--    -kernel ./arch/arm64/boot/Image \-->
<!--    --append "console=ttyAMA0" \-->
<!--    -s -S-->

<!--<!--内存模式，退出就没了-->-->
<!--qemu-system-aarch64 \-->
<!--    -machine virt \-->
<!--    -cpu cortex-a57 \-->
<!--    -machine type=virt \-->
<!--    -nographic -smp 1 \-->
<!--    -m 2048 \-->
<!--    -kernel ./linux-4.19.315/arch/arm64/boot/Image \-->
<!--    --append "console=ttyAMA0"-->


qemu-system-aarch64 \
    -M virt \
    -cpu cortex-a57 \
    -kernel ./linux4/Image \
    -hda ./linux4/rootfs.ext4 \
    -append "root=/dev/vda console=ttyAMA0 nokaslr" \
    -nographic \
    -m 4096 \
    -smp 4 \
    -net nic -net user,hostfwd=tcp::2222-:22,hostfwd=tcp::8080-:8080,hostfwd=tcp::8000-:8000,hostfwd=tcp::6611-:6611
```

