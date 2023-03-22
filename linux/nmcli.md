### 网络工具

### 例子
- wifi 设备扫描
```shell
Nmcli device wifi 
```

- 连接 wifi
```shell
Nmcli device wifi connect ssid password wifipassword 

# 或

nmcli connection add type wifi con-name wifi_name  ifname interface ssid  ssid 
nmcli connection modify wifi_name wifi-sec.key-mgmt wpa_psk wifi-sec.psk wifi_password 
```

- 开启热点
```shell
nmcli device wifi hotspot ifname wlan0 con-name MyHostspot ssid MyHostspotSSID password 12345678
```

- 查看当前 wifi 连接信息
```shell
nmcli device wifi show 
```

- 激活连接
```shell
nmcli connection up wifi_name 
```

- 删除连接配置
```shell
nmcli connection del wifi_name 
```

- 停止连接
```shell
nmcli connection down wifi_name 
```
