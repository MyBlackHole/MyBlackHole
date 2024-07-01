# losetup


```shell
losetup [-d][-e <加密方式>][-o <平移数目>][循环设备代号][文件]
```

```shell
dd if=/dev/zero of=floppy.img bs=512 count=2880

losetup /dev/loop1 floppy.img

mount /dev/loop0 /tmp


umount /tmp

losetup -d /dev/loop1
```
