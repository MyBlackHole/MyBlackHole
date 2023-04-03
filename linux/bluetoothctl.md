# bluetoothctl
蓝牙管理工具

## 命令
- 扫描蓝牙设备
```shell
bluetoothctl scan on
```

- 使自己的蓝牙被其他设备发现
```sh
bluetoothctl discoverable on
```

- 配对设备
```sh
bluetoothctl pair XX:XX:XX:XX:XX:XX
```

- 连接设备（已配对过）
```sh
bluetoothctl connect XX:XX:XX:XX:XX:XX
```

- 列出已配对设备
```sh
bluetoothctl devices
```

- 信任\取消信任设备
```sh
# 信任
bluetoothctl trust XX:XX:XX:XX:XX:XX

# 不信任
bluetoothctl untrust XX:XX:XX:XX:XX:XX
```

- 取消蓝牙配对
```sh
bluetoothctl remove FC:69:47:7C:9D:A3
```

- 断开连接
```sh
bluetoothctl disconnect FC:69:47:7C:9D:A3
```

- 阻止特定设备连接自己
```sh
bluetoothctl block FC:69:47:7C:9D:A3
```

## 服务管理

- 状态
```shell
systemctl status bluetooth
```

- 自启动
```sh
systemctl enable bluetooth
```

- 启动
```sh
systemctl start bluetooth
```
