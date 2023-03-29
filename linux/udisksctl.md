# udisksctl
用户设备管理工具

## 例子
- 挂载
```shell
udisksctl mount -b /dev/sda1
```

- 卸载
```
udisksctl unmount -b /dev/sda1
```

- 断电 (先卸载)
```shell
udisksctl power-off -b /dev/sda1
```
