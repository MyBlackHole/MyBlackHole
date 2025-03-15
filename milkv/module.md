# module

```shell
cd build
source mikvsetup.sh
defconfig cv1800b_milkv_duo_sd

build_all

<!-- 打包 img -->
pack_sd_image

ssh root@192.168.42.1

<!-- 注意 app libc 库是否存在 -->
```


```shell
podman run -itd --name milkv-duo -v $(pwd):/home/work milkvtech/milkv-duo:latest /bin/bash

SDK_DIR = /home/work
KERN_DIR = $(SDK_DIR)/linux_5.10/build/cv1800b_milkv_duo_sd

all:
	make -C $(KERN_DIR) M=$(PWD) modules

clean:
	make -C $(KERN_DIR) M=$(PWD) modules clean
	rm -rf modules.order

obj-m += hello.o

podman exec -it duodocker /bin/bash
cd /home/work
<!--export MILKV_BOARD=milkv-duo-->
<!--source milkv/boardconfig-milkv-duo.sh-->
<!--source build/milkvsetup.sh-->

source device/milkv-duo-sd/boardconfig.sh

source build/milkvsetup.sh
defconfig cv1800b_milkv_duo_sd
cd app/hello 
make

```

- 修改内核配置
```shell
root@3fc581495dcc:/home/work# source device/milkv-duo-sd/boardconfig.sh
root@3fc581495dcc:/home/work# source build/milkvsetup.sh

  -------------------------------------------------------------------------------------------------------
    Usage:
    (1) menuconfig - Use menu to configure your board.
        ex: $ menuconfig

    (2) defconfig $CHIP_ARCH - List EVB boards($BOARD) by CHIP_ARCH.
       ** cv181x ** -> ['cv181x', 'cv1811h', 'cv1811c', 'cv1810c', 'cv1810h', 'cv1812cp', 'cv1812h', 'cv1813h']
       ** cv180x ** -> ['cv180x', 'cv1800b', 'cv1800c', 'cv1801b', 'cv1801c', 'cv180zb']
        ex: $ defconfig cv183x

    (3) defconfig $BOARD - Choose EVB board settings.
        ex: $ defconfig cv1835_wevb_0002a
        ex: $ defconfig cv1826_wevb_0005a_spinand
        ex: $ defconfig cv181x_fpga_c906
  -------------------------------------------------------------------------------------------------------
root@3fc581495dcc:/home/work#
root@3fc581495dcc:/home/work# defconfig cv1800b_milkv_duo_sd
 Run defconfig function
Loaded configuration '/home/work/build/boards/cv180x/cv1800b_milkv_duo_sd/cv1800b_milkv_duo_sd_defconfig'
No change to configuration in '.config'
Loaded configuration '.config'
No change to minimal configuration in '/home/work/build/.defconfig'
/home/work/build /home/work
/home/work

====== Environment Variables =======

  PROJECT: cv1800b_milkv_duo_sd, DDR_CFG=ddr2_1333_x16
  CHIP_ARCH: CV180X, DEBUG=0
  SDK VERSION: musl_riscv64, RPC=0
  ATF options: ATF_KEY_SEL=default, BL32=1
  Linux source folder:linux_5.10, Uboot source folder: u-boot-2021.10
  CROSS_COMPILE_PREFIX: riscv64-unknown-linux-musl-
  ENABLE_BOOTLOGO: 0
  Flash layout xml: /home/work/build/boards/cv180x/cv1800b_milkv_duo_sd/partition/partition_sd.xml
  Sensor tuning bin: sony_imx307
  Output path: /home/work/install/soc_cv1800b_milkv_duo_sd


clean_all
menuconfig_kernel
build_all

```
