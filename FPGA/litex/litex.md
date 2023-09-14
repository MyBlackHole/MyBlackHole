# litex

```shell
git clone git@github.com:litex-hub/litex-boards.git
cd litex-boards

pip install migen
pip install litex
pip install litedram
python3 -m litex_boards.targets.sipeed_tang_nano_20k --build --load
```

```shell
git clone git@github.com:litex-hub/linux-on-litex-vexriscv.git
wget https://raw.githubusercontent.com/enjoy-digital/litex/master/litex_setup.py
chmod +x litex_setup.py
./litex_setup.py --init --install
<!-- ./litex_setup.py --init --install --user -->

wget https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.1.0-2019.01.0-x86_64-linux-ubuntu14.tar.gz
tar -xvf riscv64-unknown-elf-gcc-8.1.0-2019.01.0-x86_64-linux-ubuntu14.tar.gz

<!-- export PATH=$PATH:/opt/riscv64-unknown-elf-gcc-8.1.0-2019.01.0-x86_64-linux-ubuntu14/bin/ -->
export PATH=/opt/riscv64-unknown-elf-gcc-8.1.0-2019.01.0-x86_64-linux-ubuntu14/bin/:$PATH

sudo apt install libtool automake pkg-config libusb-1.0-0-dev
git clone https://github.com/ntfreak/openocd.git
cd openocd
./bootstrap
./configure --enable-ftdi
make
sudo make install

mkdir gowin
cd gowin
<!-- wget -P ~/gowin/ http://cdn.gowinsemi.com.cn/Gowin_V1.9.8.11_Education_linux.tar.gz -->
wget http://cdn.gowinsemi.com.cn/Gowin_V1.9.8.11_Education_linux.tar.gz
export PATH=/media/black/Data/lib/gowin/IDE/bin/:$PATH

python3 -m litex_boards.targets.sipeed_tang_nano_20k --build

<!-- openFPGALoader 烧录工具 -->
git clone git@github.com:trabucayre/openFPGALoader.git
cd openFPGALoader
mkdir build
cmake ..
make 
sudo make install

cargo install ecpdap
```

- 烧写FPGA固件
```shell
openFPGALoader -f -c ft2232 ./firmware/sipeed_tang_nano_20k.fs
```

- 加载Linux镜像
```shell
litex_term --speed 3000000 --serial-boot --images ./image/boot.json /dev/ttyUSB1
```

- 模拟
```shell
cd images
unzip /media/black/Data/Downloads/zip/linux_2022_03_23.zip
cd ..
pyenv activate milkv
./sim.py
```

- 硬件
```shell

<!-- 构建 -->
./make.py --board=Sipeed_tang_nano_20k --cpu-count=1 --build

<!-- toolchain -->
./make.py --board=arty --toolchain=symbiflow --build


<!-- 载入 -->
./make.py --board=Sipeed_tang_nano_20k --cpu-count=1 --load
```
