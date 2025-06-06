# qemu-system-x86_64

- 启动
```shell
qemu-system-x86_64 \
    -kernel ./linux4/bzImage \
    -hda ./linux4/rootfs.ext4 \
    -append "root=/dev/sda console=ttyS0 nokaslr" \
    -nographic \
    -m 8096 \
    -smp 4 \
    -hdb ./linux4/zfs_block.img \
    -hdc ./linux4/xfs_block.img \
    -enable-kvm \
    -net nic -net user,hostfwd=tcp::2222-:22,hostfwd=tcp::6611-:6611
```

- 启动 qcow2 镜像
```shell
qemu-system-x86_64 \
    -drive file=openEuler-20.03-LTS-SP2-x86_64.qcow2,format=qcow2 \
    -m 4096 \
    -smp 4 \
    -enable-kvm \
    -nographic \
    -netdev user,id=net0,hostfwd=tcp::2222-:22 \
    -device virtio-net-pci,netdev=net0

项目	值
用户名	root
密码	openEuler12#$

```


