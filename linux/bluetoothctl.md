# bluetoothctl
蓝牙管理工具

### 描述
**蓝牙管理工具**

### 服务
```shell
# 开启自启
sudo systemctl enable bluetooth 
# 启动服务
sudo systemctl start bluetooth 
```

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

### 语法
| 参数    | 说明           |
| ------- | -------------- |
| scan on | 扫描附近的设备 |
| pair    | 配对设备       |
| connect | 连接设备       |
|paired-devices |列出已经配对的设备  |
|devices  |列出计算机蓝牙范围内的设备   |
|trust   |信任设备  |
|untrust    |不信任设备  |
|remove     |取消配对 |
|disconnect      |取消连接设备 |
### 例子
| 命令                                      | 说明                       |
| ----------------------------------------- | -------------------------- |
| bluetoothctl scan on                      | 扫描附近的设备             |
| bluetoothctl pair XX:XX:XX:XX:XX:XX       | 配对设备                   |
| bluetoothctl connect XX:XX:XX:XX:XX:XX    | 连接已配对设备             |
| bluetoothctl paired-devices               | 列出已经配对的设备         |
| bluetoothctl devices                      | 列出计算机蓝牙范围内的设备 |
| bluetoothctl trust XX:XX:XX:XX:XX:XX      | 信任设备                   |
| bluetoothctl untrust XX:XX:XX:XX:XX:XX    | 不信任设备                 |
| bluetoothctl remove XX:XX:XX:XX:XX:XX     | 取消配对                   |
| bluetoothctl disconnect XX:XX:XX:XX:XX:XX | 取消连接设备               |
| bluetoothctl block XX:XX:XX:XX:XX:XX      | 阻止指定设备连接到系统     |

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
