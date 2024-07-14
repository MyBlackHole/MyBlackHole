# qemu-img

- 格式转换
```shell
qemu-img convert -f raw -O qcow2 archlinux-x86_64.iso archlinux-x86_64.qcow2
```

- 创建虚拟磁盘
```shell
qemu-img create ubuntu-20.04.5-amd64.img 10G

qemu-img create -f raw block.img 320M
```
