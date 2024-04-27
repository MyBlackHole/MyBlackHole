# build

- podman 环境搭建与测试
```shell
(官方)
podman run -itd --name milkv-duo -v $(pwd):/home/work milkvtech/milkv-duo:latest /bin/bash

```

- 多步骤构建
```shell
<!-- podman start hopeful_liskov -->
<!-- 环境配置 -->
export MILKV_BOARD=milkv-duo
source milkv/boardconfig-milkv-duo.sh
source build/milkvsetup.sh
  -------------------------------------------------------------------------------------------------------
    Usage:
    (1) menuconfig - Use menu to configure your board.
        ex: $ menuconfig

    (2) defconfig $CHIP_ARCH - List EVB boards($BOARD) by CHIP_ARCH.
       ** cv183x ** -> ['cv1829', 'cv1832', 'cv1835', 'cv1838', 'cv9520', 'cv7581']
       ** cv182x ** -> ['cv1820', 'cv1821', 'cv1822', 'cv1823', 'cv1825', 'cv1826', 'cv7327', 'cv7357']
       ** cv181x ** -> ['cv181x', 'cv1823a', 'cv1821a', 'cv1820a', 'cv1811h', 'cv1811c', 'cv1810c', 'cv1810h', 'cv1812cp', 'cv1812h', 'cv1813h']
       ** cv180x ** -> ['cv180x', 'cv1800b', 'cv1800c', 'cv1801b', 'cv1801c', 'cv180zb']
        ex: $ defconfig cv183x

    (3) defconfig $BOARD - Choose EVB board settings.
        ex: $ defconfig cv1835_wevb_0002a
        ex: $ defconfig cv1826_wevb_0005a_spinand
        ex: $ defconfig cv181x_fpga_c906
  -------------------------------------------------------------------------------------------------------

<!-- 生成默认配置 -->
defconfig cv1800b_milkv_duo_sd
 Run defconfig function
Loaded configuration '/root/duo-buildroot-sdk/build/boards/cv180x/cv1800b_milkv_duo_sd/cv1800b_milkv_duo_sd_defconfig'
No change to configuration in '.config'
Loaded configuration '.config'
No change to minimal configuration in '/root/duo-buildroot-sdk/build/.defconfig'
~/duo-buildroot-sdk/build ~/duo-buildroot-sdk
~/duo-buildroot-sdk

====== Environment Variables =======

  PROJECT: cv1800b_milkv_duo_sd, DDR_CFG=ddr2_1333_x16
  CHIP_ARCH: CV180X, DEBUG=0
  SDK VERSION: musl_riscv64, RPC=0
  ATF options: ATF_KEY_SEL=default, BL32=1
  Linux source folder:linux_5.10, Uboot source folder: u-boot-2021.10
  CROSS_COMPILE_PREFIX: riscv64-unknown-linux-musl-
  ENABLE_BOOTLOGO: 0
  Flash layout xml: /root/duo-buildroot-sdk/build/boards/cv180x/cv1800b_milkv_duo_sd/partition/partition_sd.xml
  Sensor tuning bin: sony_imx307
  Output path: /root/duo-buildroot-sdk/install/soc_cv1800b_milkv_duo_sd


<!-- 清除构建 -->
clean_all

<!-- 构建:多次执行增量 -->
build_all

<!-- 出包 -->
pack_sd_image
生成的固件位置: `install/soc_cv1800b_milkv_duo_sd/milkv-duo.img`
```

