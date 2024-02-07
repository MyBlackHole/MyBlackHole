# openFPGALoader

烧录工具

- 安装
```shell
sudo apt install \
  git \
  gzip \
  libftdi1-2 \
  libftdi1-dev \
  libhidapi-hidraw0 \
  libhidapi-dev \
  libudev-dev \
  zlib1g-dev \
  cmake \
  pkg-config \
  make \
  g++

git clone git@github.com:trabucayre/openFPGALoader.git
cd openFPGALoader
mkdir build
cmake -DCMAKE_INSTALL_PREFIX=/media/black/Data/lib/openFPGALoader/master \
    -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
    -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
    -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" ..
make 
make install
<!-- cargo install ecpdap -->
```

- 检测板卡
```shell
❯ sudo ./bin/openFPGALoader --detect
empty
No cable or board specified: using direct ft2232 interface
Jtag frequency : requested 6.00MHz   -> real 6.00MHz
index 0:
        idcode 0x81b
        manufacturer Gowin
        family GW2A
        model  GW2A(R)-18(C)
        irlength 8
```

- 下载流
```shell
<!-- -b 表示目标板型号，具体可以参考下面表格 -->
<!-- -f 表示下载到 flash，不加的话会下载到 sram 中 -->
<!-- 最后的是需要烧录的文件，应该找到对应目录下的 .fs 文件 -->
❯ sudo ./bin/openFPGALoader -b tangnano20k -f /media/black/Data/Documents/github/Glsl/TangNano-20K-example/hdmi/hdmi.fs
empty
write to flash
Jtag frequency : requested 6.00MHz   -> real 6.00MHz
Parse file Parse /media/black/Data/Documents/github/Glsl/TangNano-20K-example/hdmi/hdmi.fs:
Done
DONE
after program flash: displayReadReg 00006020
        Memory Erase
        Done Final
        Security Final
Erase SRAM DONE
Jtag probe limited to %d MHz6000000
Jtag frequency : requested 10.00MHz  -> real 6.00MHz
Detected: Winbond W25Q64 128 sectors size: 64Mb
Detected: Winbond W25Q64 128 sectors size: 64Mb
RDSR : 00
WIP  : 0
WEL  : 0
BP   : 0
TB   : 0
SRWD : 0
00000000 00000000 00000000 00
Erasing: [==================================================] 100.00%
Done
Writing: [==================================================] 100.00%
Done

```
- 其中-b表示目标板型，可以使用以下取值：

|Board name|FPGA|Memory|Flash|
|---|---|---|---|
|tangnano|GW1N-1 QN48|OK|Internal Flash|
|tangnano1k|GW1NZ-1 QN48|OK|Internal Flash|
|tangnano4k|GW1NSR-4C QN48|OK|Internal Flash|
|tangnano9k|GW1NR-9C QN88P|OK|Internal Flash|
|tangnano20k|GW2AR-18C QN88|OK|External Flash|
|tangprimer20k|GW2A-18C BGA256|OK|External Flash|
