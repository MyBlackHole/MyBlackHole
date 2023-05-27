# esp
开发板

## 代码数据源
```shell
git clone https://github.com/espressif/esp-idf.git
```
## esp32 s3
```shell
cd ~/esp/esp-idf
./install.sh esp32s3
. ./export.sh

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
