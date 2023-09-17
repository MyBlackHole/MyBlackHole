# buildroot


```shell
git clone http://github.com/buildroot/buildroot
cd buildroot
make BR2_EXTERNAL=../linux-on-litex-vexriscv/buildroot/ litex_vexriscv_defconfig
make


make BR2_EXTERNAL=../linux-on-litex-vexriscv/buildroot/ litex_vexriscv_usbhost_defconfig
make


The binaries are located in *output/images/* and *images/*.
```
