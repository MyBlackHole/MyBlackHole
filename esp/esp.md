# esp

开发板

usb_ttl 的 TX 接开发板 RX
usb_ttl 的 RX 接开发板 TX

## git
```shell
git clone https://github.com/espressif/esp-idf.git
```

## install

```shell
paru -S esp-idf

To use ESP-IDF:

1. Run /opt/esp-idf/install.sh to install ESP-IDF to ~/.espressif.
   You only have to do this once after installing or upgrading
   the esp-idf package.

2. Run `source /opt/esp-idf/export.sh` to add idf.py and idf_tools.py
   to your current PATH. You will have to do this in every terminal
   where you want to use ESP-IDF. Alternatively, you can add
   "source /opt/esp-idf/export.sh" to ~/.bashrc or the equivalent for
   your shell to load ESP-IDF automatically in all terminals.

/opt/esp-idf/install.sh

# 构建部分
source /opt/esp-idf/export.sh
idf.py build
```

## 案例
### esp32 s3
```shell
## 载入环境变量
cd ~/esp/esp-idf
./install.sh esp32s3
. ./export.sh

<!--或-->

source /opt/esp-idf/export.sh

## 开启串口操作权限
sudo usermod -a -G uucp $USER
groups $USER
newgrp uucp

## 切到项目下
### 设置板子
idf.py set-target esp32s3

### 配置
idf.py menuconfig

### 构建
idf.py build

### 烧录
idf.py flash (-p PORT)


### 监视输出
idf.py monitor (-p PORT -b xxxxx)

### 添加 esp-box 到当前项目中
idf.py add-dependency esp-box
```
