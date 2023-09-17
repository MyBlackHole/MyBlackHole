# milkv
小型支持 linux 开发板


- SDK (ubuntu 22.04, 大于版本或许编译有问题, 可以用容器方式)
```shell
git clone https://github.com/milkv-duo/duo-buildroot-sdk.git
cd duo-buildroot-sdk/
./build_milkv.sh
```

- 多步骤构建
```shell
export MILKV_BOARD=milkv-duo
source milkv/boardconfig-milkv-duo.sh

source build/milkvsetup.sh
defconfig cv1800b_milkv_duo_sd
clean_all
build_all
pack_sd_image

生成的固件位置: `install/soc_cv1800b_milkv_duo_sd/milkv-duo.img`
```

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
