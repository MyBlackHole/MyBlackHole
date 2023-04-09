# mknod
创建块设备、字符设备特殊文件

## 例子

- 字符设备
```shell
# 主设备号12、从设备号2
mknod /dev/cdev c 12 2
```

- 块设备
```shell
mknod /dev/bdev b 12 2
```

- 管道文件
```shell
mknod pipeFile p
```
