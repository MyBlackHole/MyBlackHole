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

- 重启 (先卸载)
```shell
udisksctl power-cycle -b /dev/sda1
```
- 格式化
```shell
udisksctl format -b /dev/sda1
```

- 显示所有设备
```shell
udisksctl info -b /dev/sda1
```
