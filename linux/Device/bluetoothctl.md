### 描述
**蓝牙管理工具**

### 服务
```shell
# 开启自启
sudo systemctl enable bluetooth 
# 启动服务
sudo systemctl start bluetooth 
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
| bluetoothctl block XX:XX:XX:XX:XX:XX      | 阻止指定设备连接到系统                           |
