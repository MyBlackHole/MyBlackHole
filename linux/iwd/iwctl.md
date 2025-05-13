# iwctl

- connect
```shell
iwctl

device list
wanl0

device wanl0 set-property Powered on
adapter adapter set-property Powered on
station wanl0 scan
station wanl0 get-networks
station wanl0 connect SSID
```

## useful commands
```shell
device list

station wlan0 connect SSID

<!-- 使用该网卡扫描附近的 wifi 热点 -->
station wlan0 scan

<!-- 打印 wifi 热点信息 -->
station wlan0 get-networks
```
