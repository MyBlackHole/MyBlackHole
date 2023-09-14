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
export PATH=$PATH:$PWD/riscv64-unknown-elf-gcc-8.1.0-2019.01.0-x86_64-linux-ubuntu14/bin/
```
