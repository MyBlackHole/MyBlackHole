# build

- docker 环境搭建与测试
```shell
git clone git@github.com:pavelanni/milkv-duo-experiments.git
cd milkv-duo-experiments/build-container/

<!-- Containerfile -->
FROM ubuntu:22.04
RUN apt-get update
# the following is take from here: https://stackoverflow.com/questions/44331836/apt-get-install-tzdata-noninteractive
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install tzdata
# RUN apt install -y tzdata
# this is according to the instructions from here: https://github.com/milkv-duo/duo-buildroot-sdk#readme
RUN apt install -y pkg-config build-essential ninja-build automake autoconf libtool wget curl git gcc libssl-dev bc slib squashfs-tools android-sdk-libsparse-utils jq python3-distutils scons parallel tree python3-dev python3-pip device-tree-compiler ssh cpio fakeroot libncurses5 flex bison libncurses5-dev genext2fs rsync unzip dosfstools mtools tcl openssh-client cmake vim
RUN wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1-1ubuntu2.1~18.04.23_amd64.deb
RUN dpkg -i libssl1.1_1.1.1-1ubuntu2.1~18.04.23_amd64.deb
# RUN git clone https://github.com/milkv-duo/duo-buildroot-sdk.git /root/duo-buildroot-sdk
# WORKDIR /root/duo-buildroot-sdk
# RUN wget https://sophon-file.sophon.cn/sophon-prod-s3/drive/23/03/07/16/host-tools.tar.gz -O host-tools.tar.gz
# RUN tar xzf host-tools.tar.gz
# # need this to run Go http server: https://forum.sophgo.com/t/duo-golang/211
# RUN sed -i 's/CONFIG_EPOLL=.*$/CONFIG_EPOLL=y/' build/boards/cv180x/cv1800b_milkv_duo_sd/linux/cvitek_cv1800b_milkv_duo_sd_defconfig
# # need this to free the memory used for camera
# RUN sed -i 's/ION_SIZE = .*/ION_SIZE = 0 * SIZE_1M/' build/boards/cv180x/cv1800b_milkv_duo_sd/memmap.py
WORKDIR /root
CMD /bin/bash


docker build -t milkv-duo-build:0.1.0 -f Containerfile .

<!-- mkdir -p ~/out -->
<!-- export OUTPUT_DIR=~/out -->

<!-- pwd 代表 duo-buildroot-sdk 当前目录路径 -->
docker run -it -v `pwd`:/root/duo-buildroot-sdk:z milkv-duo-build:0.1.0
./build_milkv.sh
```

- 多步骤构建
```shell
<!-- docker start hopeful_liskov -->
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

